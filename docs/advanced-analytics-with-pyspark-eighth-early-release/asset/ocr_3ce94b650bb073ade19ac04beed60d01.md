# TRANSFORMATIONS AND ACTIONS

The act of creating a DataFrame does not cause any distributed computation to take place on the cluster. Rather, DataFrames define logical data sets that are intermediate steps in a computation. Spark operations on distributed data can be classified into two types: transformations and actions.

All transformations are evaluated lazily. That is, their results are not computed immediately, but they are recorded as a lineage. This allows Spark to optimize the query plan. Distributed computation occurs upon invoking an _action_ on a DataFrame. For example, the `count` action returns the number of objects in an DataFrame:

```
df.count()
...
15
```

The `collect` action returns an `Array` with all the `Row` objects from the DataFrame. This `Array` resides in local memory, not on the cluster:

```
df.collect()
[Row(id='12', department='sales'), ...
```

Actions need not only return results to the local process. The `save` action saves the contents of the DataFrame to persistent storage:

```
df.write.format("parquet").("user/ds/mynumbers")
```

Remember that `DataFrameReader` can accept a directory of text files as input, meaning that a future Spark job could refer to `mynumbers` as an input directory.

# Analyzing Data with the DataFrame API

The DataFrame API comes with a powerful set of tools that will likely be familiar to data scientists who are used to Python, and SQL. In this section, we