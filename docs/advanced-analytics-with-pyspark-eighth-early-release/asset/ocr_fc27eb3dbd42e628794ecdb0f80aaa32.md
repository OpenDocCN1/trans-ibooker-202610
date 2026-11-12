```
+-----------------+-----------+
| 1092764|1000311|
| 1095122|1000557|
| 6708070|1007267|
|10088054|1042317|
| 1195917|1042317|
+-----------------+-----------+
```

The first entry maps ID 1092764 to 1000311. We can look these up from
artist_by_id DataFrame:

```
artist_by_id.filter(artist_by_id.id.isin(1092764,
1000311)).show()
...
```

| id | name |
|---|---|
| 1000311 | Steve Winwood |
| 1092764 | Winwood, Steve |

This entry evidently maps “Winwood, Steve” to “Steve Winwood,” which
is in fact the correct name for the artist.

# # Building a First Model

Although the data set is in nearly the right form for use with Spark MLlib's
ALS implementation, it requires a small extra transformation. The aliases
data set should be applied to convert all artist IDs to a canonical ID, if a
different canonical ID exists.

```
from pyspark.sql.functions import broadcast, when
train_data = train_data =
user_artist_df.join(broadcast(artist_alias), 'artist',
how='left') \
.withColumn('artist', when(col('alias').isNull(),
col('artist')).otherwise(col('alias'))) \
.withColumn('artist',
col('artist').cast(IntegerType())).drop('alias')
```