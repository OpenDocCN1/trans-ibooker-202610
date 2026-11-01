best practices for Amazon Athena usage for optimizing cost and improving performance for SQL-based analysis:
https://aws.amazon.com/blogs/big-data/top-10-performance-tuning-tips-for-amazon-athena/

+   - Athena for Spark supported Apache Spark version 3.2 when it was launched. Refer to the Athena documentation for the support of recent Spark versions. For additional details about Athena for Spark, refer to the following blog: .

# When to use SQL/Spark analysis options?

If you want to perform analytics on your data lake without loading it into another storage layer, use Athena SQL to catalog and query the data. You can also use CTAS statements to perform lightweight data transformations. If you want to execute Spark programs in an interactive and serverless manner for some of your analysis use cases, the Athena for Spark option can be used.

# Advanced data discovery and data structuring with Athena

In this section, we will explore how Amazon Athena helps in performing advanced data discovery and data structuring phases of the data wrangling pipeline.

# SQL-based data discovery with Athena

Amazon Athena provides SQL-based data exploration and supports advanced data types as well. The advantage here is that even people with no coding expertise can explore data with familiar SQL syntax on multiple formats of data and different storage types.

Before we query data using Amazon Athena, we need to create metadata for the table in the Glue catalog. Amazon Athena helps you to query data from different data sources, as follows:

+   - **AWS Glue catalog**—Access tables that are available in the AWS Glue catalog. We can create tables in the AWS Glue catalog in multiple ways, which we will explore later in this chapter.
+   - **Federated data sources**—Supports querying data from Apache Hive, AWS, and third-party databases and services. For an extensive list of supported data sources, refer to https://docs.aws.amazon.com/athena/latest/ug/connectors-prebuilt.html.

In addition to prebuilt connectors, you can also create custom connectors to query the data source of your choice. We will explore in detail federated data sources further in this chapter.

Let us understand more about the Glue catalog, which will be the central metadata store for most Athena queries. The Glue catalog is a persistent metadata store that lets users store, annotate, and share metadata. There will be a unique Glue catalog per region within each AWS account. You can use **Identity and Access Management (IAM)** policies to define and control access to users for the