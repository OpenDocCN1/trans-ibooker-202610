screenshot, you can see information such as when the query started and finished, where the results are stored, the format of the file, and so on.

```
result_type
: results.query_metadata
: {'QueryExecutionId': 'a551b95f-c7e6-4423-b3ef-a2994f05e7f6',
'Query': 'CREATE TABLE "default"."temp_table_1712a308609b46218042dcbb4286ab3d"\nWITH(\n    external_location = \'s3://aws-athena-query-results-473373155991-us-east-2/temp_table_1712a308609b46218042dcbb4286ab3d\',\n    format = \'PARQUE T\')\nAS SELECT date,time,bytes,method,uri,result_type from cloudfront_logs limit 1',
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

+   - **TotalExecutionTime**: The number of ms that Athena took to run a DDL or DML query.
+   - **EngineExecutionTime**: The number of ms that the Athena engine took to run the query. The more complex the query and the larger the data volume, the longer this phase takes.
+   - **QueryPlanningTime**: The number of ms that Athena took to plan the query processing flow.
+   - **QueryQueueTime**: The number of ms that the query was in the query queue waiting for resources. This is a variable time and depends on the availability of resources at the specific Region and time.
+   - **ServiceProcessingTime**: The number of ms that Athena took to process the query results after the query engine finished running the query. If a query spends too much time in this phase, we write a big dataset to S3 and try to reduce the size of the resulting set.