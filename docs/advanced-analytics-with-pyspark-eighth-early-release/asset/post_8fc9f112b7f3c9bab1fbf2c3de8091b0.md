
seems that $k$ could be at least 23, or likely even more. Typically, many values of $k$ are tried to find the best one. But what is “best”?

A clustering could be considered good if each data point were near its closest centroid, where “near” is defined by the Euclidean distance. This is a simple, common way to evaluate the quality of a clustering, by the mean of these distances over all points, or sometimes, the mean of the distances squared. In fact, KMeansModel offers a computeCost method that computes the sum of squared distances and can easily be used to compute the mean squared distance.

It’s simple enough to manually evaluate the clustering cost for several values of $k$. Note that this code could take 10 minutes or more to run.

```
from pyspark.sql import DataFrame
from pyspark.ml.evaluation import ClusteringEvaluator

from random import randint

def clustering_score(input_data, k):
    input_numeric_only = input_data.drop("protocol_type",
    "service", "flag")
    assembler =
    VectorAssembler().setInputCols(input_numeric_only.columns[:-1]).setOutputCol("featureVector")
    kmeans =
    KMeans().setSeed(randint(100,10000)).setK(k).setPredictionCol("cluster").setFeaturesCol("featureVector")
    pipeline = Pipeline().setStages([assembler, kmeans])
    pipeline_model = pipeline.fit(input_numeric_only)

    evaluator = ClusteringEvaluator(predictionCol='cluster',
    featuresCol="featureVector")
    predictions = pipeline_model.transform(numeric_only)
    score = evaluator.evaluate(predictions)
    return score

for k in list(range(20,100, 20)):
    print(clustering_score(numeric_only, k))
```

```
(20, 6.649218115128446E7)
(40, 2.5031424366033625E7)
(60, 1.027261913057096E7)
(80, 1.251413171109027E7)
(100, 7235531.565096531)
```

scores will be shown here using scientific notation

- 1. The printed result shows that the score decreases as $k$ increases. Note that scores are shown in scientific notation; the first value is over $10^7$, not just a bit over 6.

> Again, your values will be somewhat different. The clustering depends on a randomly chosen initial set of centroids.

However, this much is obvious. As more clusters are added, it should always be possible to put data points closer to the nearest centroid. In fact, if $k$ is chosen to equal the number of data points, the average distance will be 0 because every point will be its own cluster of one!

Worse, in the preceding results, the distance for $k=80$ is higher than for $k=60$. This shouldn't happen because higher $k$ always permits at least as good a clustering as a lower $k$. The problem is that K-means is not necessarily able to find the optimal clustering for a given $k$. Its iterative process can converge from a random starting point to a local minimum, which may be good but is not optimal.

This is still true even when more intelligent methods are used to choose initial centroids. K-means++ and K-means|| are variants of selection algorithms that are more likely to choose diverse, separated centroids and lead more reliably to a good clustering. Spark MLlib, in fact, implements K-means||. However, all still have an element of randomness in selection and can't guarantee an optimal clustering.

The random starting set of clusters chosen for $k=80$ perhaps led to a particularly suboptimal clustering, or it may have stopped early before it reached its local optimum.

We can improve it by running the iteration longer. The algorithm has a threshold via `setTol()` that controls the minimum amount of cluster centroid movement considered significant; lower values mean the K-means algorithm will let the centroids continue to move longer. Increasing the maximum number of iterations with `setMaxIter()` also prevents it from potentially stopping too early at the cost of possibly more computation.

```
def clustering_score_1(input_data, k):
    input_numeric_only = input_data.drop("protocol_type",
    "service", "flag")
    assembler =
    VectorAssembler().setInputCols(input_numeric_only.columns[:-1]).setOutputCol("featureVector")
    kmeans =
    KMeans().setSeed(randint(100,100000)).setK(k).setMaxIter(40).\
    setTol(1.0e-5).\
    setPredictionCol("cluster").setFeaturesCol("featureVector")
    pipeline = Pipeline().setStages([assembler, kmeans])
    pipeline_model = pipeline.fit(input_numeric_only)
    #
    evaluator = ClusteringEvaluator(predictionCol='cluster',
    featuresCol="featureVector")
    predictions = pipeline_model.transform(numeric_only)
    score = evaluator.evaluate(predictions)
    #
    return score
```

```
for k in list(range(20, 101, 20)):
    print(k, clustering_score_1(numeric_only, k))
```

Increase from default 20

Decrease from default 1.0e-4

This time, at least the scores decrease consistently:

```
(20, 1.8041795813813403E8)
(40, 6.33056876207124E7)
(60, 9474961.544965891)
(80, 9388117.93747141)
(100, 8783628.926311461)
```

We want to find a point past which increasing k stops reducing the score much—or an “elbow” in a graph of k versus score, which is generally decreasing but eventually flattens out. Here, it seems to be decreasing notably past 100. The right value of k may be past 100.

## Visualization with SparkR

At this point, it could be useful to step back and understand more about the data before clustering again. In particular, looking at a plot of the data points could be helpful.

Spark itself has no tools for visualization, but the popular open source statistical environment R has libraries for both data exploration and data visualization. Furthermore, Spark also provides some basic integration with R via SparkR. This brief section will demonstrate using R and SparkR to cluster the data and explore the clustering.

SparkR is a variant of the spark-shell used throughout this book, and is invoked with the command sparkR. It runs a local R interpreter, like spark-shell runs a variant of the Scala shell as a local process. The machine that runs sparkR needs a local installation of R, which is not included with Spark. This can be installed, for example, with sudo apt-get install r-base on Linux distributions like Ubuntu, or brew install R with Homebrew on macOS.

SparkR is a command-line shell environment, like R. To view visualizations, it’s necessary to run these commands within an IDE-like environment that can display images. RStudio is an IDE for R (and works with SparkR); it runs on a desktop operating system so it will only be usable here if you are experimenting with Spark locally rather than on a cluster.

If you are running Spark locally, download the free version of RStudio and install it. If not, then most of the rest of this example can still be run with sparkR on a command line; for example, on a cluster. It won’t be possible to display visualizations this way though.

If running via RStudio, launch the IDE and configure SPARK_HOME and JAVA_HOME, if your local environment does not already set them, to point to the Spark and JDK installation directories, respectively.

```
Sys.setenv(SPARK_HOME = "/path/to/spark") ①
Sys.setenv(JAVA_HOME = "/path/to/java")
library(SparkR, lib.loc = c(file.path(Sys.getenv("SPARK_HOME"),
"R", "lib")))
sparkR.session(master = "local[*]",
sparkConfig = list(spark.driver.memory = "4g"))
```

Replace with actual paths, of course.

Note that these steps aren't needed if you are running sparkR on the command line. Instead, it accepts command-line configuration parameters like --driver-memory, just like spark-shell.

SparkR is an R-language wrapper around the same DataFrame and MLlib APIs that have been demonstrated in this chapter. It's therefore possible to recreate a K-means simple clustering of the data:

```
clusters_data <- read.df("/path/to/kddcup.data", "csv", ①
inferSchema = "true", header = "false")
```

```
colnames(clusters_data) <- c( ②
"duration", "protocol_type", "service", "flag",
"src_bytes", "dst_bytes", "land", "wrong_fragment", "urgent",
"hot", "num_failed_logins", "logged_in", "num_compromised",
"root_shell", "su_attempted", "num_root", "num_file_creations",
"num_shells", "num_access_files", "num_outbound_cmds",
"is_host_login", "is_guest_login", "count", "srv_count",
"serror_rate", "srv_serror_rate", "error_rate",
"srv_error_rate",
"same_srv_rate", "diff_srv_rate", "srv_diff_host_rate",
"dst_host_count", "dst_host_srv_count",
"dst_host_same_srv_rate", "dst_host_diff_srv_rate",
"dst_host_same_src_port_rate", "dst_host_srv_diff_host_rate",
"dst_host_serror_rate", "dst_host_srv_serror_rate",
"dst_host_error_rate", "dst_host_srv_error_rate",
"label")
```

```
numeric_only <- cache(drop(clusters_data, ③
c("protocol_type", "service", "flag",
"label")))
```
