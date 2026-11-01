```
%time
#Path variable here is the S3 staging path for loading to Redshift
wr.redshift.copy(df=speed_test_load, path="s3://[REDACTED]/awswrangler/data-staging/",
    con=con,schema="public", table="speed_test_uc2",mode="overwrite",
    iam_role="arn:aws:iam::473373155991:role/RedshiftImmersionRole")
```

CPU times: user 3.31 s, sys: 318 ms, total: 3.63 s
Wall time: 8.48 s

Figure 3.68: Redshift integration – loading data into Redshift from S3 using the COPY command

We can verify the staging file getting created in the S3 path provided in the aforementioned copy command. To be clear, we ran this statement twice, and it took 15 seconds during the first run and 8 seconds during the second run. So, the load time depends on multiple factors, including bandwidth and the capacity available at that time.

Users can query the data from Redshift and validate the new table to ensure that the changes made are loaded into it.

| avg_lat_ms | tests | devices | newdevices | avg_usedbandwidth |
|---|---|---|---|---|
| 22 | 1 | 1 | 2 | 400783 |
| 29 | 5 | 3 | 4 | 69938 |
| 31 | 2 | 1 | 2 | 5879 |

Figure 3.69: Redshift integration – the query loaded data from Redshift

As a part of this use case, we will also explore the to_sql function to write data into Redshift, but this will use INSERT statements to load the data and is not as performant as the copy command. However, this method won’t have the overhead of storing data in the S3 staging path.

```
61:
%time
#Use case – Load S3 files into Pandas, make some changes and load it to Redshift using insert command directly
speed_test=wr.redshift.to_sql(df=speed_test_load,con=con,schema="public",table="speed_test_uc3")
```

CPU times: user 1min 28s, sys: 3.19 s, total: 1min 32s
Wall time: 18min 34s

Figure 3.70: Redshift integration – loading data into Redshift using the to_sql function

You can see that this takes 18 minutes, which is much higher than the first two methodologies. Of course, this time depends on multiple factors, but this use case gives you an idea of what happens when the data is smaller, such as reference table loading.

We can further reduce the loading time by using the chunk size parameter. By default, chunk size is configured for 200 records, so records get loaded in batches of 200. This factor can be increased based on the capacity of the machine on which it’s run. The following screenshot shows the execution time with the chunk size parameter set to 2000. Still, this option takes more time compared to the copy function.