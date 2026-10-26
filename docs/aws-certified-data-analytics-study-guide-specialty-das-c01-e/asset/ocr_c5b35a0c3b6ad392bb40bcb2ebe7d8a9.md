+   - Parallelization of queries against your data in the data lake to provide better performance
+   - Sharing of data across multiple Redshift clusters

Using Amazon Redshift Lakehouse is quite simple. The three-step process is as follows:

+   1. Set up an external schema. This can be using the AWS Glue Data Catalog or an Apache Hive Metastore.

```
CREATE EXTERNAL SCHEMA <schema_name>
```

+   2. Register the external tables from the following sources:

+   - Amazon Athena
+   - Hive Metastore
+   - Amazon Redshift CREATE EXTERNAL TABLE syntax.

```
CREATE EXTERNAL TABLE <table_name>
[PARTITIONED BY <column_name, data_type, ...>]
STORED AS file_format
LOCATION s3_location
[TABLE PROPERTIES property_name=property_value, ...];
```

+   3. Start querying your tables using:

```
<schema_name>.<table_name>
```

# Data Modeling in Redshift

Redshift is a fully ACID- and ANSI-compliant data warehouse (ACID stands for atomicity, consistency, isolation, durability). Redshift doesn’t have the concept of indexes but provides the concept of distribution keys and sort keys to give you the same performance benefits as you would building an index. Performance in an MPP database is always dependent on how well the data is distributed across the cluster, and Redshift is no exception, where the choice of distribution key, sort key, and compression can significantly impact performance of the cluster.

Redshift is a relational database, which means you need to know the structure of the data before you write it to Redshift. Data inside Redshift is organized in databases (the recommendation is to have one database per Redshift cluster). A database can have one or more schemas (namespaces), which contain tables and other database objects (such as stored procedures). Tables are composed of a tabular structure in the form of rows and columns.

Redshift only supports scalar data types, which means you cannot use List or Map. While Redshift is built for relational data, it does support semi-structured data in the form of JSON or AVRO, and using Redshift Lakehouse, you can work with a variety of file formats on the Redshift data lake using a SerDes (Serializer/Deserializer).

There are five major design considerations to achieve optimal performance in Redshift. Thankfully, Amazon has now automated most of these features, which means either the