
Clusters are interesting insofar as they define regions of normal connections; everything else outside is unusual and potentially anomalous.

## KDD Cup 1999 Data Set

The KDD Cup was an annual data mining competition organized by a special interest group of the Association for Computing Machinery (ACM). Each year, a machine learning problem was posed, along with a data set, and researchers were invited to submit a paper detailing their best solution to the problem. In 1999, the topic was network intrusion, and the data set is still available at the KDD website. We will need to download the kddcupdata.data.gz and kddcup.names files from the website. The remainder of this chapter will walk through building a system to detect anomalous network traffic using Spark, by learning from this data.

> Don't use this data set to build a real network intrusion system! The data did not necessarily reflect real network traffic at the time—even if it did, it reflects traffic patterns from 22 years ago.

Fortunately, the organizers had already processed raw network packet data into summary information about individual network connections. The data set is about 708 MB in size and contains about 4.9 million connections. This is large, if not massive, and is certainly sufficient for our purposes here. For each connection, the data set contains information like the number of bytes sent, login attempts, TCP errors, and so on. Each connection is one line of CSV-formatted data set, containing 38 features. Feature information and ordering can be found in the kddcup.names file.

Unzip the kddcup.data.gz data file and copy it into your storage. This example, like others, will assume the file is available at data/kddcup.data. Let us see the data in its raw form:

```
head -n 1 data/kddcup.data
```

```
...
```

```
0,tcp,http,SF,215,45076,0,0,0,0,0,1,0,0,0,0,0,0,0,0,0,1,1,0.00,0.00,0.00,0.00,1.000,0.00,0.00,0.00,0.00,0.00,0.00,0.00,0.00,0.00,0.00,0.00,0.00,0.00,0.00,0.00,normal.
```

This connection, for example, was a TCP connection to an HTTP service— 215 bytes were sent and 45,706 bytes were received. The user was logged in, and so on.

Many features are counts, like *num_file_creations* in the 17th column, as listed in the *kddcup.names* file. Many features take on the value 0 or 1, indicating the presence or absence of a behavior, like *su_attempted* in the 15th column. They look like the one-hot encoded categorical features from *Chapter 4*, but are not grouped and related in the same way. Each is like a yes/no feature, and is therefore arguably a categorical feature. It is not always valid to translate categorical features as numbers and treat them as if they had an ordering. However, in the special case of a binary categorical feature, in most machine learning algorithms, mapping these to a numeric feature taking on values 0 and 1 will work well.

The rest are ratios like *dst_host_srv_error_rate* in the next-to-last column, and take on values from 0.0 to 1.0, inclusive.

Interestingly, a label is given in the last field. Most connections are labeled *normal*. but some have been identified as examples of various types of network attacks. These would be useful in learning to distinguish a known attack from a normal connection, but the problem here is anomaly detection and finding potentially new and unknown attacks. This label will be mostly set aside for our purposes.

## A First Take on Clustering

Open the *spark-shell*, and load the CSV data as a data frame. It’s a CSV file again, but without header information. It’s necessary to supply column names as given in the accompanying *kddcup.names* file.

```
data_without_header = spark.read.option("inferSchema",
True).option("header", False).csv("data/kddcup.data")
```

```
column_names = [ "duration", "protocol_type", "service", "flag",
"src_bytes", "dst_bytes", "land", "wrong_fragment", "urgent",
"hot", "num_failed_logins", "logged_in", "num_compromised",
"root_shell", "su_attempted", "num_root", "num_file_creations",
"num_shells", "num_access_files", "num_outbound_cmds",
"is_host_login", "is_guest_login", "count", "srv_count",
"serror_rate", "srv_serror_rate", "rerror_rate",
"srv_rerror_rate",
"same_srv_rate",
"diff_srv_rate", "srv_diff_host_rate",
"dst_host_count", "dst_host_srv_count",
"dst_host_same_srv_rate", "dst_host_diff_srv_rate",
"dst_host_same_src_port_rate", "dst_host_srv_diff_host_rate",
"dst_host_serror_rate", "dst_host_srv_serror_rate",
"dst_host_rerror_rate", "dst_host_srv_rerror_rate",
"label"]
```

```
data = data_without_header.toDF(*column_names)
```

Begin by exploring the data set. What labels are present in the data, and how many are there of each? The following code simply counts by label and prints the results in descending order by count.

```
data.select("label").groupBy("label").count().orderBy(col("count"
).desc()).show(25)
```

| label | count |
|-------|-------|
| smurf. | 2807886 |
| neptune. | 1072017 |
| normal. | 972781 |
| satan. | 15892 |
| ... | ... |
| phf. | 4 |
| perl. | 3 |
| spy. | 2 |

There are 23 distinct labels, and the most frequent are smurf. and neptune. attacks.

Note that the data contains nonnumeric features. For example, the second column may be `tcp`, `udp`, or `icmp`, but K-means clustering requires numeric features. The final label column is also nonnumeric. To begin, these will simply be ignored.

Aside from this, creating a K-means clustering of the data follows the same pattern as was seen in Chapter 4. A `VectorAssembler` creates a feature vector, a `KMeans` implementation creates a model from the feature vectors, and a `Pipeline` stitches it all together. From the resulting model, it’s possible to extract and examine the cluster centers.

```
from pyspark.ml.feature import VectorAssembler
from pyspark.ml.clustering import KMeans, KMeansModel
from pyspark.ml import Pipeline

numeric_only = data.drop("protocol_type", "service",
"flag").cache()

assembler =
VectorAssembler().setInputCols(numeric_only.columns[:-1]).setOutputCol("featureVector")

kmeans =
KMeans().setPredictionCol("cluster").setFeaturesCol("featureVector")

pipeline = Pipeline().setStages([assembler, kmeans])
pipeline_model = pipeline.fit(numeric_only)
kmeans_model = pipeline_model.stages[1]

from pprint import pprint
pprint (kmeans_model.clusterCenters ())

...
[array([4.83401949e+01, 1.83462155e+03, 8.26203190e+02,
5.71611720e-06,
6.48779303e-04, 7.96173468e-06...])),
array([1.09990000e+04, 0.00000000e+00, 1.3099374e+09,
0.0000000e+00,
0.0000000e+00, 0.0000000e+00, ...])]
```

It’s not easy to interpret the numbers intuitively, but each of these represents the center (also known as centroid) of one of the clusters that the model produced. The values are the coordinates of the centroid in terms of each of the numeric input features.

Two vectors are printed, meaning K-means was fitting k=2 clusters to the data. For a complex data set that is known to exhibit at least 23 distinct types of connections, this is almost certainly not enough to accurately model the distinct groupings within the data.

This is a good opportunity to use the given labels to get an intuitive sense of what went into these two clusters by counting the labels within each cluster.

```
with_cluster = pipeline_model.transform(numeric_only)
with_cluster.select("cluster", "label").groupBy("cluster",
"label").count() .orderBy(col("cluster"),
col("count").desc() ).show(25)
```

| cluster | label | count |
|---------|-------|-------|
| 0 | smurf. | 2807886 |
| 0 | neptune. | 1072017 |
| 0 | normal. | 972781 |
| 0 | satan. | 15892 |
| 0 | ipsweep. | 12481 |
| ... | ... | ... |
| 0 | phf. | 4 |
| 0 | perl. | 3 |
| 0 | spy. | 2 |
| 1 | portsweep. | 1 |

The result shows that the clustering was not at all helpful. Only one data point ended up in cluster 1!

## Choosing k

Two clusters are plainly insufficient. How many clusters are appropriate for this data set? It's clear that there are 23 distinct patterns in the data, so it
