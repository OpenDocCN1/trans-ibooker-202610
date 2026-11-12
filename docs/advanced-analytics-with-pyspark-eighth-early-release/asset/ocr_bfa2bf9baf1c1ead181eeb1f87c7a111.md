# 1% sample without replacement

clustering_sample is actually a local R data frame, not a Spark DataFrame, so it can be manipulated like any other data in R. Above, str() shows the structure of the data frame.

For example, it’s possible to extract the cluster assignment and then show statistics about the distribution of assignments:

```
clusters <- clustering_sample["prediction"] ①
data <- data.matrix(within(clustering_sample, rm("prediction")))
        ②
table(clusters)
...
clusters
0   11   14   18   23   25   28   30   31   33   36
...
47294   3   1   2   2  308  105   1   27 1219   15
...
```

Only the clustering assignment column
Everything but the clustering assignment

For example, this shows that most points fell into cluster 0. Although much more could be done with this data in R, further coverage of this is beyond the scope of this book.

To visualize the data, a library called rgl is required. It will only be functional if running this example in RStudio. First, install (once) and load the library:

```
install.packages("rgl")
library(rgl)
```

Note that R may prompt you to download other packages or compiler tools to complete installation, because installing the package means compiling its source code.