# TO UDF OR NOT TO UDF?

PySpark SQL makes it very easy to inline business logic into functions that can be used from standard SQL, as we did here with the `find_borough` function. Given this, you might think that it would be a good idea to move all of your business logic into UDFs in order to make it easy to reuse, test, and maintain. However, there are a few caveats for using UDFs that you should be mindful of before you start sprinkling them throughout your code

+   - First, UDFs are opaque to PySpark’s SQL query planner and execution engine in a way that standard SQL query syntax is not, so moving logic into a UDF instead of using a literal SQL expression could hurt query performance.
+   - Second, handling null values in PySpark SQL can get complicated quickly, especially for UDFs that take multiple arguments.

We can apply `find_borough` to the trips in the `taxi_raw` DataFrame to create a histogram of trips by borough:

```
df_with_boroughs = taxi_raw.withColumn("dropoff_borough",
find_borough_udf(fun.col("dropoff_latitude"),\
fun.col('dropoff_longitude')))

df_with_boroughs.groupBy(fun.col("dropoff_borough")).count().show()
()
...
```

| dropoff_borough | count |
|---|---|
| Queens | 672192 |
| null | 7942421 |
| Brooklyn | 715252 |
| Staten Island | 3338 |
| Manhattan | 12979047 |
| Bronx | 67434 |