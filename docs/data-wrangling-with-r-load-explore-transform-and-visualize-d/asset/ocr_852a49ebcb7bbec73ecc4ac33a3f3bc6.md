```
word":"testpassword", "engine":"mysql", "port":"3306"}
```

awswrangler will internally format the connection string from the secrets manager or Glue connection and use the Create a connection object (pymysql in the case of MySQL, pyodbc in the case of sqlserver, etc.) and return to the calling code. This connection object can be used from that point onward to read and write to the database.

+   - **Reading from database**: There are a couple of options to read from database engines – Read from SQL and Read from table.
+     Read from table will create a select query on the table and again call the Read from SQL module.

The query is formatted and submitted through the connection object, which is passed as an argument to the Read from SQL module. The code will execute the query using cursors from the connection object.

Once the data is retrieved from the execution, it’s loaded into a pyarrow array to enable faster loading of data from the record object in the cursor execution. We can then quickly convert it to a pandas DataFrame and return to the calling function to further process the data.

The following screenshot shows a quick example of loading the Ookla speed test data from S3 (4.1 million records) in three minutes, with the lowest instance size for the notebook and database. The intent of this exercise is to show the ease with which data can be loaded and queried with awswrangler.

```
import awswrangler as wr
import pandas as pd
```

```
dbcon= wr.postgresql.connect("Postgres-wrangler")
```

```
#load S3 data
speed_test=wr.s3.read_parquet("s3://ookla-open-data/parquet/performance/type=mobile/year=2021/quarter=4/")
```

```
len(speed_test)

4120347
```

```
%time
wr.postgresql.to_sql(speed_test, dbcon, schema="public", table="speed_test", mode="overwrite")

CPU times: user 2min 3s, sys: 2.64 s, total: 2min 5s
Wall time: 3min 7s
```

```
%time
df_result=wr.postgresql.read_sql_query("SELECT * FROM public.speed_test limit 10", con=dbcon)

CPU times: user 0 ns, sys: 4.43 ms, total: 4.43 ms
Wall time: 5.48 ms
```

df_result
| quadkey | tile | avg_d_kbps | avg_u_kbps | avg_lat_ms | tests | devices |
|---|---|---|---|---|---|---|
| 0 0022133222330013 POLYGON((-160.032348632812 70.6399478155463, ...)) | 85 | 103 | 72 | 1 | 1 |
| 1 0022133222330201 POLYGON((-160.043334960938 70.6344840663086, ...)) | 14947 | 20624 | 70 | 1 | 1 |
| 2 0022302331031201 POLYGON((-166.1517333989438 68.8714387233513, ...)) | 13099 | 1281 | 91 | 2 | 1 |

Figure 3.57: RDS integration – loading sample data from RDS