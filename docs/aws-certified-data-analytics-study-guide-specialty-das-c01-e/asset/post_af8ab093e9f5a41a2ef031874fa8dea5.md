
- Redshift automatically performs VACUUM and ANALYZE in the background during periods of low workloads, but Redshift users are still empowered to explicitly invoke VACUUM and then ANALYZE as part of their workflow after adding, deleting, or modifying a large number of rows (> 5%).
  - Explicitly invoking VACUUM and then ANALYZE ensures that a table is sorted, defragmented, and analyzed immediately and with priority for the benefit of the next step in a workflow.
- Disable column encoding analysis (COMPUPDATE) and statistics update (STATUPDATE) from COPY if loading a small amount of data relative to overall table size.
- Enforce primary, unique, or foreign key constraints outside of Amazon Redshift.
- Wrap workflow/statements in an explicit transaction.
- Consider using DROP TABLE or TRUNCATE instead of DELETE.
- When moving all rows from a table (e.g., source table to staging table, or vice versa), use the faster ALTER TABLE APPEND (which moves rather than copies data) instead of INSERT INTO SELECT.
- Staging tables:
  - Use a temporary or permanent table with the BACKUP NO option.
  - Mirror compression settings from a production table by using CREATE TABLE LIKE or using ANALYZE COMPRESSION to add the column-level compression into the DDL; also, turn off automatic compression analysis in the COPY command (i.e., COMPUPDATE OFF).
  - If applicable, use DISTSTYLE KEY on both the staging table and production table (on the same key column) to speed up an INSERT INTO SELECT statement.

## Loading Data from Other AWS Services

You can load data to Redshift from other AWS services, including but not limited to the following:

**Kinesis Data Firehose**  Kinesis Firehose can automatically load data to Redshift. It micro-batches streaming data to S3 and then uses the COPY command to load the data to Redshift using a manifest file. You can specify the batch size and the number of seconds before a load occurs. Kinesis Firehose can do multi-AZ buffering of data prior to a load and manages all compression and encryption.

**Amazon EMR**  You can load data from Amazon EMR (essentially HDFS). However, the cluster needs to be started with Redshift compatibility bootstrap action.

```
COPY <table> from 'emr://emr_cluster_id/hdfs_file_path' iam_role "arn"
```

**DynamoDB**  You can load from DynamoDB using the COPY command. However, a COPY command will count against your provisioned read capacity.

```
COPY <redshift_tablename>
from 'dynamodb://<dynamodb_table_name>' iam_role "arn" readratio
'<integer>';
```

### SSH Endpoint

You can load data to Amazon Redshift from any endpoint connected via SSH. This option is useful for SFTP, NAS, or on-premises remote hosts. It requires an SSH manifest file containing the information COPY will use to open SSH connections and execute the remote commands.

```
COPY <redshift_tablename>
from 's3://ssh_manifest_file' iam_role "arn"
SSH [| optional-parameters];
```

### AWS Lambda

You can use AWS Lambda to load to Amazon Redshift using aws-lambda-redshift-loader. The connector is available on GitHub at github.com/awslabs/aws-lambda-redshift-loader. You should consider an AWS Lambda session limit if you are planning to load data to Amazon Redshift from AWS Lambda.

### Federated Query

You can use Federated Query to query and analyze data across operational databases, data warehouses, and data lakes. This feature allows you to integrate queries on live data in Amazon RDS for PostgreSQL and Amazon Aurora PostgreSQL with queries across your Amazon Redshift and Amazon S3 environments. Redshift’s intelligent optimizer pushes down and distributes portions of computation directly into remote operational databases to reduce data moved over the network.

You can therefore incorporate live data as part of your business intelligence (BI) and reporting applications. This option makes it easy to ingest data into Redshift by querying operational databases directly, applying transformations on the fly, and loading data into target tables without complex ETL pipelines.

## Unloading Data from Amazon Redshift

Building a complete information architecture often means that you will need to unload data from Redshift on occasion. Sometimes this could be due to the fact that you want to unload data marts that were built inside Redshift but now host cold data, and at other times it could be due to other applications that would be consuming data available on your Redshift cluster.

You have three common ways to unload data from Redshift:

- AWS Glue
- AWS Data Pipeline
- UNLOAD statement

You can use the UNLOAD command to extract data from Redshift. UNLOAD is basically a reverse of the COPY statement. The UNLOAD statement outputs data from Amazon Redshift to S3 in CSV or Parquet format. The key semantics of an UNLOAD are as follows:

- Runs from a SELECT statement. Since the output is generally large and performance is a concern, the statement runs on all the compute nodes in parallel. If you would like an ordered output, you have to define ORDER BY in the SELECT statement, in addition to setting PARALLEL=OFF.
- Encryption and compression are handled automatically.
- The number of files generated is larger than the slice count, generally more than one file per slice.
- You can create a manifest file as a part of the output operation. This is especially useful if you would like to move the data to another cluster.
- You can control the maximum file size written to S3 and the overwrite semantics as to whether you would like to overwrite the output or not.

## Query Optimization in Redshift

Redshift is an ACID- and ANSI SQL–compliant cloud data warehouse and uses similar syntax to PostgreSQL while supporting stored procedures and custom UDFs with SQL SELECT or Python. A typical Redshift query passes through three major phases:

1. Planning – A user-submitted query is parsed and optimized to create a query plan.
2. Compilation – Redshift checks its compile cache to see if it can find a query-plan match. If it can find a query-plan match, it reuses the existing compiled objects; otherwise it converts it to sub-tasks that are then individually compiled into C++.
3. Execution – The compiled code is executed by the compute node slices in parallel and the results are then aggregated by the leader node to be sent back to the requestor.

A query plan is a basic tool that can help you understand and analyze a complex query. It gives you an insight into how the query will actually be run on the cluster. When the query plan is compiled, the execution engine translates the query plan into steps, segments, and streams (see Figure 4.21):

| Term | Definition |
| --- | --- |
| Step | A step is an individual operation that is required during the execution of a query. |
| Segment | A segment is a compilation of several steps that need to be done by a single process. This is the smallest compilation unit executable by the individual slice of a compute node. The compute node slices will execute the segments in parallel. |
| Stream | A stream is a collection of segments to be parceled out over the available compute node slices. You can have multiple segments in a stream, and the segments are then run in parallel. |

**Figure 4.21 Relationship between streams, segments, and steps**

![](img/fc6a7083d65b0716921d7bd4a5298460_258_0.png)

### Explain Plans in Redshift

You can generate explain plans using the EXPLAIN statement, which allows you to generate a pre-formatted output with one row per step and a single column indicating the step. If you use Redshift Query Editor, you can generate a visual explain plan. Explain plans are read from the bottom up, and they provide information like the following:

- Execution order of steps for local and Lakehouse portions
- Estimated number of rows to be returned by a step
- Amount of data to be sent across slices per step (Network I/O is expensive and should be optimized as much as possible.)
- Relative cost of a step

You can look at historic trends of explain plans generated for queries that were previously run on the system using STL_EXPLAIN table. One of the key components in understanding explain plans is understanding the terminology being used during the explain plan generation. As discussed earlier, Redshift is an MPP database, where many compute nodes work together to produce efficient results, which means that the network becomes a really important component during data processing, especially join processing. During a particular step in a join, you may need to work on data that is not stored locally and hence data from a different slice on the same node, or in the worst-case scenario, a different node may need to be fetched. Since network transmission is by far the most expensive operation a query will do, it is important to understand how data is moving across the nodes. Table 4.15 describes the terminology that you will see in explain plans.
