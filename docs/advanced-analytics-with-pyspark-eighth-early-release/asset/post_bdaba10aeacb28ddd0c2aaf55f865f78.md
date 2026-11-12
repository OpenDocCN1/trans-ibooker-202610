
of a 0/1 value indicating whether the user won't or will interact with the artist, respectively.

After extracting the artist IDs for the recommendations, we can look up artist names in a similar way:

```
top_predictions_pandas = top_predictions.toPandas()
print(top_predictions_pandas)
...
             user                    recommendations
0    2093760   [(2814, 0.029410675168037415), (1300642, 0.028...
...
recommended_artist_ids = [i[0] for i in
top_predictions_pandas.recommendations[0]]

artist_by_id.filter(col('id').isin(recommended_artist_ids)).show()
...
```

| id | name |
|---|---|
| 2814 | 50 Cent |
| 4605 | Snoop Dogg |
| 1007614 | Jay-Z |
| 1001819 | 2Pac |
| 1300642 | The Game |

The result is all hip-hop. This doesn't look like a great set of recommendations, at first glance. While these are generally popular artists, they don't appear to be personalized to this user's listening habits.

## Evaluating Recommendation Quality

Of course, that's just one subjective judgment about one user's results. It's hard for anyone but that user to quantify how good the recommendations are. Moreover, it's infeasible to have any human manually score even a small sample of the output to evaluate the results.

It's reasonable to assume that users tend to play songs from artists who are appealing, and not play songs from artists who aren't appealing. So, the plays for a user give a partial picture of “good” and “bad” artist recommendations. This is a problematic assumption but about the best that can be done without any other data. For example, presumably user 2093760 likes many more artists than the 5 listed previously, and among the 1.7 million other artists not played, a few are of interest and not all are “bad” recommendations.

What if a recommender were evaluated on its ability to rank good artists high in a list of recommendations? This is one of several generic metrics that can be applied to a system that ranks things, like a recommender. The problem is that “good” is defined as “artists the user has listened to,” and the recommender system has already received all of this information as input. It could trivially return the user’s previously listened-to artists as top recommendations and score perfectly. But this is not useful, especially because the recommender’s role is to recommend artists that the user has never listened to.

To make this meaningful, some of the artist play data can be set aside and hidden from the ALS model-building process. Then, this held-out data can be interpreted as a collection of good recommendations for each user but one that the recommender has not already been given. The recommender is asked to rank all items in the model, and the ranks of the held-out artists are examined. Ideally, the recommender places all of them at or near the top of the list.

We can then compute the recommender’s score by comparing all held-out artists’ ranks to the rest. (In practice, we compute this by examining only a sample of all such pairs, because a potentially huge number of such pairs may exist.) The fraction of pairs where the held-out artist is ranked higher is its score. A score of 1.0 is perfect, 0.0 is the worst possible score, and 0.5 is the expected value achieved from randomly ranking artists.

This metric is directly related to an information retrieval concept called the receiver operating characteristic (ROC) curve. The metric in the preceding paragraph equals the area under this ROC curve, and is indeed known as AUC, or Area Under the Curve. AUC may be viewed as the probability that a randomly chosen good recommendation ranks above a randomly chosen bad recommendation.

The AUC metric is also used in the evaluation of classifiers. It is implemented, along with related methods, in the MLlib class `BinaryClassificationMetrics`. For recommenders, we will compute AUC _per user_ and average the result. The resulting metric is slightly different, and might be called “mean AUC.” We will implement this, because it is not (quite) implemented in PySpark.

Other evaluation metrics that are relevant to systems that rank things are implemented in `RankingMetrics`. These include metrics like precision, recall, and mean average precision (MAP). MAP is also frequently used and focuses more narrowly on the quality of the top recommendations. However, AUC will be used here as a common and broad measure of the quality of the entire model output.

In fact, the process of holding out some data to select a model and evaluate its accuracy is common practice in all of machine learning. Typically, data is divided into three subsets: training, cross-validation (CV), and test sets. For simplicity in this initial example, only two sets will be used: training and CV. This will be sufficient to choose a model. In `Chapter 4`, this idea will be extended to include the test set.

## Computing AUC

An implementation of mean AUC is provided in the source code accompanying this book. It is not reproduced here, but is explained in some detail in comments in the source code. It accepts the CV set as the “positive” or “good” artists for each user, and a prediction function. This function translates a data frame containing each user-artist pair into a data frame that also contains its estimated strength of interaction as a “prediction,” a number wherein higher values mean higher rank in the recommendations.

In order to use it, we must split the input data into a training and CV set. The ALS model will be trained on the training data set only, and the CV set will be used to evaluate the model. Here, 90% of the data is used for training and the remaining 10% for cross-validation:

```python
def area_under_curve(
    positive_data,
    b_all_artist_ids,
    predict_function):
...

all_data = user_artist_df.join(broadcast(artist_alias), 'artist',
how='left') \
.withColumn('artist', when(col('alias').isNull(),
col('artist')).otherwise(col('alias'))) \
.withColumn('artist',
col('artist').cast(IntegerType())).drop('alias')

train_data, cv_data = all_data.randomSplit([0.9, 0.1],
seed=54321)
train_data.cache()
cv_data.cache()

all_artist_ids = all_data.select("artist").distinct().count()
b_all_artist_ids = broadcast(all_artist_ids)

model = ALS(rank=10, seed=0, maxIter=5, regParam=0.1,
implicitPrefs=True, alpha=1.0, userCol='user', itemCol='artist',
ratingCol='count') \
.fit(train_data)
area_under_curve(cv_data, b_all_artist_ids, model.transform)
```

Note that areaUnderCurve() accepts a function as its third argument. Here, the transform method from ALSModel is passed in, but it will shortly be swapped out for an alternative.

The result is about 0.879. Is this good? It is certainly higher than the 0.5 that is expected from making recommendations randomly, and it’s close to 1.0, which is the maximum possible score. Generally, an AUC over 0.9 would be considered high.

But is it an accurate evaluation? This evaluation could be repeated with a different 90% as the training set. The resulting AUC values’ average might be a better estimate of the algorithm’s performance on the data set. In fact, one common practice is to divide the data into *k* subsets of similar size, use *k*–1 subsets together for training, and evaluate on the remaining subset. We can repeat this *k* times, using a different set of subsets each time. This is called *k*-fold *cross-validation*. This won’t be implemented in examples here, for simplicity, but some support for this technique exists in MLlib in its *CrossValidator* API. The validation API will be revisited in “Random Forests”.

It’s helpful to benchmark this against a simpler approach. For example, consider recommending the globally most-played artists to every user. This is not personalized, but it is simple and may be effective. Define this simple prediction function and evaluate its AUC score:

```python
from pyspark.sql.functions import sum as _sum

def predict_most_listened(train):
    listen_counts = train.groupBy("artist").agg(_sum("count").alias("prediction")).select("artist", "prediction")
    return all_data.join(listen_counts, "artist", "left_outer").select("user", "artist", "prediction")

area_under_curve(cv_data, b_all_artist_ids,
predict_most_listened(train_data))
```

The result is also about 0.880. This suggests that nonpersonalized recommendations are already fairly effective according to this metric. However, we’d expect the “personalized” recommendations to score better in comparison. Clearly, the model needs some tuning. Can it be made better?
