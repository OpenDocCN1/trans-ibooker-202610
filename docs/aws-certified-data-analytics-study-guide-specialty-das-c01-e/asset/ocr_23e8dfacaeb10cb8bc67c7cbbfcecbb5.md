# ## Shuffle in Spark

Certain operations in Spark trigger an event known as a *shuffle*. A shuffle is one of the most complex and costly operations because it involves copying the data across the executors and machines, typically over a network. The operation involves disk I/O, data serialization, and network I/O, and hence special care needs to be taken whenever an operation requires a shuffle. The following operations cause a shuffle in Spark:

+   - Repartition
+   - Coalesce()
+   - All ByKey operations, such as GroupByKey(), ReduceByKey() (except Counts)
+   - Cogroup()
+   - Join()

# ## DataFrames and Datasets API in Spark

A dataset is a distributed collection of data (added in Spark 1.6) that provides the benefits of RDDs with the added advantage of Spark SQL’s optimized execution engine. Spark 2.0 combines the DataFrame and Dataset API into one Dataset API. The entry point to Spark SQL is SparkSession from Spark 2.0. Each item is of the type ROW. For example, a DataFrame that contains two columns, column A and column B, with integers looks like DataFrame(ROW(A=25,B=30), ROW(A=3,B=9)).

The DataFrame concept in Spark is inspired by dataframes in Python and R; however, it is a better implementation than that of R and Python from a performance standpoint.

Spark uses the RDD API as the fundamental data structure. All the computations are done in RDDs as the fundamental building block in Spark. Starting in Spark 2.0, the Dataset takes on two distinct APIs characteristics:

+   - A strongly typed API
+   - An untyped API

Since Python and R have no compile-time type safety, we only have untyped APIs (DataFrames) and no Dataset APIs in R and Python.

RDDs provide strong compile-time type safety (*type safety* is the extent to which a programming language prevents type errors) and provide object-oriented operations. RDDs can use high-level expressions like lambda functions (anonymous function not related to AWS Lambda) and map function. The disadvantage of an RDD is in its performance due to massive overhead from garbage collection (creation and destruction of JVM objects) and serializing individual objects.

The *DataFrame* API gives a relational and structured view of the data, allowing Spark to manage the schema and pass data between nodes without using the Java serializer. Spark DataFrame uses the Catalyst optimizer to optimize the query execution plan and executing queries. Spark DataFrame also uses a Tungsten execution backend to improve Spark execution by optimizing Spark jobs for CPU and memory efficiency.

Spark DataFrame can use SQL expressions like group, filter, and join on the data as well. The disadvantage of the DataFrame API is that it does not provide a strong compile-time type safety.