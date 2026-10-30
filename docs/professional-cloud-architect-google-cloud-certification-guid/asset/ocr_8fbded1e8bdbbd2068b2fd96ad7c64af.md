6. Now, we can compose a new query on this dataset. This query will provide some valuable information. We will retrieve both the average and the highest tip given to drivers, grouped by drop-off communities. We can see that this query uses standard SQL language and that ANSI 2011 is supported:

```
SELECT
dropoff_community_area,
FORMAT('%3.2f', AVG(tips)) AS average_tip,
FORMAT('%3.2f', MAX(tips)) AS max_tip
FROM
`bigquery-public-data.chicago_taxi_trips.taxi_trips`
WHERE
dropoff_community_area IS NOT NULL
GROUP BY
dropoff_community_area
ORDER BY
average_tip DESC
LIMIT
10
```

![](bbox=[0.22, 0.17, 0.76, 0.66])

Figure 13.26 – Dataset query

Notice from the results that we have processed the query in only 1.2 seconds. This may not be a large amount of data that's been queried, but it shows that we can quickly load a dataset and execute our query. There were no prerequisite steps to deploying the infrastructure.

One thing regarding BigQuery that we should mention is that it has a command-line tool called bq. We will look at bq in more detail in Chapter 16, Google Cloud Management Options.