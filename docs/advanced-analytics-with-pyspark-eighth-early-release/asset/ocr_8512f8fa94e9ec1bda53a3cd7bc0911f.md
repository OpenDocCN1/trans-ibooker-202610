# COMPARISON WITH MAPREDUCE

One illuminating way to understand Spark is in terms of its advances over its predecessor, Apache Hadoop’s MapReduce. MapReduce revolutionized computation over huge data sets by offering a simple and resilient model for writing programs that could execute in parallel across hundreds to thousands of machines. It breaks up work into small *tasks* and can gracefully accommodate task failures without compromising the job to which they belong.

Spark maintains MapReduce’s linear scalability and fault tolerance, but extends it in three important ways.

+   - First, rather than relying on a rigid map-then-reduce format, its engine can execute a more general directed acyclic graph (DAG) of operators. This means that in situations where MapReduce must write out intermediate results to the distributed filesystem, Spark can pass them directly to the next step in the pipeline.
+   
+   - Second, it complements its computational capability with a rich set of transformations that enable users to express computation more naturally. Out-of-the-box functions are provided for various tasks including numerical computation, datetime processing and string manipulation.
+   
+   - Third, Spark extends its predecessors with in-memory processing. This means that future steps that want to deal with the same data set need not recompute it or reload it from disk. Spark is well suited for highly iterative algorithms as well as adhod queries.

# PySpark

PySpark is Spark’s Python API. In simpler words, PySpark is a Python-based wrapper over the core Spark framework which is written primarily in Scala. PySpark provides an intuitive programming environment for data