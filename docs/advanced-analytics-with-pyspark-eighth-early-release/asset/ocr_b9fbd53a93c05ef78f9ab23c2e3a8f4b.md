produced. The values are the coordinates of the centroid in terms of each of the numeric input features.

Two vectors are printed, meaning K-means was fitting k=2 clusters to the data. For a complex data set that is known to exhibit at least 23 distinct types of connections, this is almost certainly not enough to accurately model the distinct groupings within the data.

This is a good opportunity to use the given labels to get an intuitive sense of what went into these two clusters by counting the labels within each cluster.

```
with_cluster = pipeline_model.transform(numeric_only)
with_cluster.select("cluster", "label").groupBy("cluster",
"label").count() .orderBy(col("cluster"),
col("count").desc() ).show(25)
```

...
+---+-----+-------+
| cluster | label | count |
+---+-----+-------+
| 0 | smurf.| 2807886 |
| 0 | neptune.| 1072017 |
| 0 | normal.| 972781 |
| 0 | satan.| 15892 |
| 0 | ipsweep.| 12481 |
...
| 0 | phf.| 4 |
| 0 | perl.| 3 |
| 0 | spy.| 2 |
| 1 | portsweep.| 1 |
+---+-----+-------+

The result shows that the clustering was not at all helpful. Only one data point ended up in cluster 1!

# Choosing k

Two clusters are plainly insufficient. How many clusters are appropriate for this data set? It's clear that there are 23 distinct patterns in the data, so it