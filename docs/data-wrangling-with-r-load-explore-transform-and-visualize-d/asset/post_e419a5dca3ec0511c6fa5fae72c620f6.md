
the following logic to evaluate whether a query has to be executed or retrieve results from the previous execution in a faster manner.

The following screenshot shows some simple steps involved in creating a Glue table using `awswrangler` and querying the table using it. We can set configuration variables once during program execution, and the queries are executed quickly without a lot of parameters and additional coding.

```
speed_test=wr.s3.read_parquet("s3://ookla-open-data/parquet/performance/type=mobile/year=2021/quarter=4/")
```

```
speed_test.dtypes
```

```
quadkey      string
tile         string
avg_d_kbps   Int64
avg_u_kbps   Int64
avg_lat_ms   Int64
tests        Int64
devices      Int64
dtype: object
```

```
wr.catalog.create_parquet_table(
    database='default',
    table='speed_test',
    path='s3://ookla-open-data/parquet/performance/',
    partitions_types={'type': 'string','year': 'string','quarter': 'string'},
    columns_types={'quad_key':'string','tile':'string','avg_d_kbps':'bigint','avg_u_kbps':'bigint','avg_lat_ms':'bigint'},
    description='Speed test table'
)
```

```
%time
wr.athena.read_sql_query("SELECT * from speed_test limit 1")
```

CPU times: user 261 ms, sys: 12.2 ms, total: 274 ms
Wall time: 553 ms

| quad_key | tile | avg_d_kbps | avg_u_kbps | avg_lat_ms | tests | devices | type | year | quarter |
|---|---|---|---|---|---|---|---|---|---|
| 0 <NA> POLYGON((-160.043334960938 70.6363054807905, ... |  | 908 | 4915 | 72 | 5 | 1 | mobile | 2019 | 2 |

Figure 3.50: Athena integration – creating a table in the AWS Glue catalog

The following are the steps involved in fetching data from an `awswrangler` query/table API call:

- 1. When you submit a read SQL table API call internally, the table name gets appended with a select statement and gets executed through a read SQL query API call.
- 2. There are a lot of arguments to configure the working of query submission. We can use the `params` argument to format the query.
- 3. Evaluate whether the query is available in the local cache (which gets updated with every new query execution and stores 100 queries by default). This step also looks for queries from the remote cache (the Athena service query history) based on configuration parameters, including past X queries or past X seconds.
- 4. If there is no hit in the cache step, the query is submitted for execution by `awswrangler` in either Create Table as Statement (CTAS), UNLOAD, or regular mode:
  - **CTAS mode**: This is the default mode, and it involves wrapping a query within a `create table as` statement, with data stored in an S3 location in the Parquet format. This method also creates a table with the output of the query in a CTAS database, which is provided as an input argument. The Parquet file is then loaded into a DataFrame using the `awswrangler` S3 read parquet file API. Also, the temporary tables will be deleted at the end of the query execution to avoid cluttering the tables from each query. Use this for bigger datasets where performance is necessary and it also needs permissions to create and drop temporary CTAS tables.
  - **Unload mode:** This mode involves unloading data from a query into an S3 location as a Parquet file. After that, similar to CTAS, we will load the dataset into a pandas DataFrame using the `awswrangler` S3 read parquet file API. Use this for bigger datasets and data that is exported to S3 in the Parquet format. Note that this method doesn’t create Glue tables before loading in a DataFrame.
  - **Regular mode:** This mode uses the Athena output location where the results are saved in the CSV file format. This method is simple and doesn’t require any additional configuration to execute queries. Use this for smaller datasets, as they are stored in the CSV file format in S3. The files are stored by default using Athena and, hence, don’t require any additional storage or configuration requirements.

The following diagram summarizes the `awswrangler` Athena query execution flow:

## HIGH LEVEL QUERY EXECUTION FLOW FOR ATHENA IN AWSWRANGLER

![](img/ec1933a597ce63afa3bcf7496364f412_134_0.png)

Figure 3.51: Athena integration – the query execution workflow

## How to utilize query metadata for performance

The pandas DataFrame also has a `query_metadata` dictionary, which is similar to the output of the `GetQueryExecution` command from Athena that retrieves each query metadata. In the following screenshot, you can see information such as when the query started and finished, where the results are stored, the format of the file, and so on.

```
result_type
: results.query_metadata
: {'QueryExecutionId': 'a551b95f-c7e6-4423-b3ef-a2994f05e7f6',
'Query': 'CREATE TABLE "default"."temp_table_1712a308609b46218042dcbb4286ab3d"\nWITH(\n    external_location = \'s3://aws-athena-query-results-473373155991-us-east-2/temp_table_1712a308609b46218042dcbb4286ab3d\',\n    format = \'PARQUET\')\nAS SELECT date,time,bytes,method,uri,result_type from cloudfront_logs limit 1',
'StatementType': 'DDL',
'ResultConfiguration': {'OutputLocation': 's3://aws-athena-query-results-473373155991-us-east-2/tables/a551b95f-c7e6-4423-b3ef-a2994f05e7f6'},
'QueryExecutionContext': {'Database': 'default'},
'Status': {'State': 'SUCCEEDED'},
'SubmissionDateTime': datetime.datetime(2022, 10, 4, 1, 29, 33, 922000, tzinfo=tzlocal()),
'CompletionDateTime': datetime.datetime(2022, 10, 4, 1, 29, 35, 338000, tzinfo=tzlocal()),
'Statistics': {'EngineExecutionTimeInMillis': 1289,
'DataScannedInBytes': 1495,
'DataManifestLocation': 's3://...-manifest.csv',
'TotalExecutionTimeInMillis': 1416,
'QueryQueueTimeInMillis': 106,
'QueryPlanningTimeInMillis': 156,
'ServiceProcessingTimeInMillis': 21},
'WorkGroup': 'primary',
'EngineVersion': {'SelectedEngineVersion': 'AUTO'},
'EffectiveEngineVersion': 'Athena engine version 2'}
```

Figure 3.52: Athena integration – getting query metadata from Athena

However, we also have a bunch of important attributes under the “Statistics” segment. They provide information on the query wait time, the planning time, the execution time, and also the data scanned. Note that Athena queries are charged based on the data scanned:

```
TotalExecutionTime = EngineExecutionTime(includes QueryPlanningTime) + QueryQueueTime + ServiceProcessingTime
```

Here is an example from the preceding case – 1,416 ms = 1,289 ms + 106 ms + 21 ms.

The EngineExecution time was 1,289 ms, out of which Queryplanningtime was 156 ms. So, the actual execution time for the query in the Athena engine was 1,133ms.

Let's understand the terms used in the preceding query here:

- - **TotalExecutionTime**: The number of ms that Athena took to run a DDL or DML query.
- - **EngineExecutionTime**: The number of ms that the Athena engine took to run the query. The more complex the query and the larger the data volume, the longer this phase takes.
- - **QueryPlanningTime**: The number of ms that Athena took to plan the query processing flow.
- - **QueryQueueTime**: The number of ms that the query was in the query queue waiting for resources. This is a variable time and depends on the availability of resources at the specific Region and time.
- - **ServiceProcessingTime**: The number of ms that Athena took to process the query results after the query engine finished running the query. If a query spends too much time in this phase, we write a big dataset to S3 and try to reduce the size of the resulting set.

You can use these metrics to look at where each query spends time and then optimize them. However, note that these metrics don’t include any network latencies and delays outside of the Athena query engine.

## How does Athena cache work?

We can enable cache logic by setting the `MAX_CACHE_SECONDS` variable to be greater than zero. This will help decrease the query execution time (by skipping the execution entirely and reusing the past execution results if they are still available).

The following steps are evaluated while performing cache validation:

- 1. Check whether the `MAX_CACHE_SECONDS` variable is greater than zero; if it’s set to zero or less, return immediately and the queries are executed every time they are called. This is needed for use cases where data is updated more frequently and you require recent data rather than lower query execution times.
- 2. Retrieve queries based on `MAX_REMOTE_CACHE_ENTRIES` from the Athena history log using pagination. Check whether the query details are available in the local cache; if not, append the query details to the local cache object. The local cache stores the data about the most recent 100 queries so that the data can be retrieved much faster than the remote cache. In this `awswrangler` Athena context, the cache can be understood as the storage for query metadata details, such as the query execution ID and query completion time.
- 3. Sort the list of queries from the local cache and the remote cache based on the condition that the elapsed seconds query completion time is not greater than `MAX_CACHE_SECONDS`.
- 4. Parse the query so that a successful match can be made with older queries. Currently, the query is stripped of white space and trailing semicolons and leading and trailing brackets are removed so that the queries can be matched to a reasonable degree of confidence.
- 5. If the older query is CTAS (executed with `awswrangler` CTAS mode), then parse and take the inner query before it can be compared with a new query. Compare both queries, and if there is a match, return the cache match as a success.

For example, let us execute a simple query that extracts data from an example table, crawled on CloudFront logs and stored in an S3 bucket. You could use any table for this demonstration of Athena cache functionality.

```
%time
wr.athena.read_sql_query("SELECT date,time,bytes,method,uri,referrer,result_type from cloudfront_logs limit 1")
CPU times: user 298 ms, sys: 12.7 ms, total: 311 ms
Wall time: 3.42 s

date    time    bytes method uri referrer result_type
0   2022-02-18 16:12:03 481 GET / - Error
```

```
%time
wr.athena.read_sql_query("SELECT date,time,bytes,method,uri,referrer,result_type from cloudfront_logs limit 1;")
CPU times: user 87.3 ms, sys: 2.99 ms, total: 90.2 ms
Wall time: 333 ms

date    time    bytes method uri referrer result_type
0   2022-02-18 16:12:03 481 GET / - Error
```

Figure 3.53: Athena integration – Athena cache functionality
