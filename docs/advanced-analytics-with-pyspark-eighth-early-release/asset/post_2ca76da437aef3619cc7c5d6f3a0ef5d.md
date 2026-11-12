
around the time of the Netflix Prize competition by papers like “Collaborative Filtering for Implicit Feedback Datasets” and “Large-Scale Parallel Collaborative Filtering for the Netflix Prize”. PySpark MLlib’s ALS implementation draws on ideas from both of these papers and is the only recommender algorithm currently implemented in Spark MLlib.

Here’s a code snippet (non-functional) to give you a peek at what lies ahead in the chapter.

```python
from pyspark.ml.recommendation import ALS

als = ALS(maxIter=5, regParam=0.01, userCol="user",
itemCol="artist", ratingCol="count")
model = als.fit(train)

predictions = model.transform(test)
```

With ALS, we will treat our input data as a large sparse matrix A, and find out X and Y, as discussed in previous section. At the start, Y isn’t known, but it can be initialized to a matrix full of randomly chosen row vectors. Then simple linear algebra gives the best solution for X, given A and Y. In fact, it’s trivial to compute each row i of X separately as a function of Y and of one row of A. Because it can be done separately, it can be done in parallel, and that is an excellent property for large-scale computation:

A_i Y(Y^T Y)^{-1} = X_i

Equality can’t be achieved exactly, so in fact the goal is to minimize |A_i Y(Y^T Y)^{-1} - X_i|, or the sum of squared differences between the two matrices’ entries. This is where the “least squares” in the name comes from. In practice, this is never solved by actually computing inverses but faster and more directly via methods like the QR decomposition. This equation simply elaborates on the theory of how the row vector is computed.

The same thing can be done to compute each Y_j from X. And again, to compute X from Y, and so on. This is where the “alternating” part comes from. There’s just one small problem: Y was made up, and random! X was computed optimally, yes, but given a bogus solution for Y. Fortunately, if this process is repeated, X and Y do eventually converge to decent solutions.

When used to factor a matrix representing implicit data, there is a little more complexity to the ALS factorization. It is not factoring the input matrix A directly, but a matrix P of 0s and 1s, containing 1 where A contains a positive value and 0 elsewhere. The values in A are incorporated later as weights. This detail is beyond the scope of this book, but is not necessary to understand how to use the algorithm.

Finally, the ALS algorithm can take advantage of the sparsity of the input data as well. This, and its reliance on simple, optimized linear algebra and its data-parallel nature, make it very fast at large scale.

Next, we will preprocess our dataset and make it suitable for use with the ALS algorithm.

## Preparing the Data

The first step in building a model is to understand the data that is available, and parse or transform it into forms that are useful for analysis in Spark.

Spark MLlib's ALS implementation does not strictly require numeric IDs for users and items, but is more efficient when the IDs are in fact representable as 32-bit integers. That is the case because under the hood, the data is being represented using the Java Virtual Machine's (JVM) data type. Does this data set conform to this requirement already?

```python
raw_user_artist_data.show(10)
```

| value |
|---|
| 1000002 1 55 |
| 1000002 1000006 33 |
| 1000002 1000007 8 |
| 1000002 1000009 144 |
| 1000002 1000010 314 |
| 1000002 1000013 8 |
| 1000002 1000014 42 |
| 1000002 1000017 69 |
| 1000002 1000024 329 |
| 1000002 1000025 1 |

Each line of the file contains a user ID, an artist ID, and a play count, separated by spaces. To compute statistics on the user ID, we split the line by space characters, and parse the values as integers. The result is conceptually three “columns”: a user ID, artist ID and count as Ints. It makes sense to transform this to a data frame with columns named “user”, “artist”, and “count” because it then becomes simple to compute simple statistics like the maximum and minimum:

```python
from pyspark.sql.functions import split, min, max
from pyspark.sql.types import IntegerType, StringType
user_artist_df = raw_user_artist_data.withColumn('user',
    split(raw_user_artist_data['value'], ' ').getItem(0).cast(IntegerType())) \
    .withColumn('artist', split(raw_user_artist_data['value'], ' ').getItem(1).cast(IntegerType())) \
    .withColumn('count', split(raw_user_artist_data['value'], ' ').getItem(2).cast(IntegerType())).drop('value')
user_artist_df.select([min("user"), max("user"), min("artist"), max("artist")]).show()
```

| min(user) | max(user) | min(artist) | max(artist) |
|---|---|---|---|
| 90 | 2443548 | 1 | 10794401 |

The maximum user and artist IDs are 2443548 and 10794401, respectively (and their minimums are 90 and 1; no negative values). These are comfortably smaller than 2147483647. No additional transformation will be necessary to use these IDs.

It will be useful later in this example to know the artist names corresponding to the opaque numeric IDs. raw_artist_data contains the artist ID and name separated by a tab. PySpark’s split function accepts regular expression values for the `pattern` parameter. We can split using the whitespace character, `\s`:

```python
from pyspark.sql.functions import col

artist_by_id = raw_artist_data.withColumn('id',
    split(col('value'), '\\s+', 2).getItem(0).cast(IntegerType())) \
    .withColumn('name',
    split(col('value'), '\\s+', 2).getItem(1).cast(StringType())) \
    .drop('value')
```

| id | name |
|---|---|
| 1134999 | 06Crazy Life |
| 6821360 | Pang Nakarin |
| 10113088 | Terfel, Bartoli- ... |
| 10151459 | The Flaming Sidebur |
| 6826647 | Bodenstandig 3000 |

This gives a data frame with the artist ID and name as columns “id” and “name”.

raw_artist_alias maps artist IDs that may be misspelled or nonstandard to the ID of the artist’s canonical name. This dataset is relatively small, containing about 200,000 entries. It contains two IDs per line, separated by a tab. We will parse this in a similar manner as we did raw_artist_data.

```python
artist_alias = raw_artist_alias.withColumn('artist',
    split(col('value'), '\\s+').getItem(0).cast(IntegerType())) \
    .withColumn('alias',
    split(col('value'), '\\s+').getItem(1).cast(StringType())) \
    .drop('value')
```

| artist | alias |
|---|---|
| 1092764 | 1000311 |
| 1095122 | 1000557 |
| 6708070 | 1007267 |
| 10088054 | 1042317 |
| 1195917 | 1042317 |

The first entry maps ID 1092764 to 1000311. We can look these up from artist_by_id DataFrame:

```python
artist_by_id.filter(artist_by_id.id.isin(1092764, 1000311)).show()
...
```

| id | name |
|---|---|
| 1000311 | Steve Winwood |
| 1092764 | Winwood, Steve |

This entry evidently maps “Winwood, Steve” to “Steve Winwood,” which is in fact the correct name for the artist.

## Building a First Model

Although the data set is in nearly the right form for use with Spark MLlib's ALS implementation, it requires a small extra transformation. The aliases data set should be applied to convert all artist IDs to a canonical ID, if a different canonical ID exists.

```python
from pyspark.sql.functions import broadcast, when
train_data = user_artist_df.join(broadcast(artist_alias), 'artist', how='left') \
    .withColumn('artist', when(col('alias').isNull(), col('artist')).otherwise(col('alias'))) \
    .withColumn('artist', col('artist').cast(IntegerType())).drop('alias')
```

```python
train_data.cache()
train_data.count()
...
24296858
```

Get artist's alias if it exists, otherwise get original artist.

We `broadcast` the `artist_alias` DataFrame created earlier. This makes Spark send and hold in memory just one copy for *each executor* in the cluster. When there are thousands of tasks and many execute in parallel on each executor, this can save significant network traffic and memory. As a rule of thumb, it's helpful to broadcast a significantly smaller dataset when performing a join with a very big dataset.
