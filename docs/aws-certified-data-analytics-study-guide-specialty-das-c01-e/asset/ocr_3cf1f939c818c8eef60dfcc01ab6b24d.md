# Worker Nodes

A cluster typically contains many worker nodes that run one or more executors, which process the work assigned by the driver program.

# Executors

The executors are responsible for actually performing the jobs as assigned by the driver program. Each executor executes the code as requested by the driver program and then reports the status of the execution back to the driver node. The executors will be running the Spark code.

# Tasks

Tasks are the actual individual running of the code. It is important to understand how tasks are created and then scheduled across the cluster. Spark will first create a graph of your execution (such as an RDD graph) and then create a plan of execution. If you look at your driver program for a basic word count, you will see multiple stages against which multiple tasks are created and are scheduled across the worker nodes. Typically, tasks are created based on how your data is distributed.

To learn more about the architecture, please visit Apache Spark documentation at bit.ly/2tNHCmD.

# Spark Programming Model

As you can see from the previous section, Spark has a master-slave architecture, with multiple worker nodes and executors running the code in parallel to optimize the processing. Spark introduces the concept of Resilient Distributed Datasets (RDDs), which is basically a pointer/abstraction of the data distributed and cached in memory across the Spark cluster nodes. The concept of RDDs has two major attributes:

+   - Resilient – An RDD is an abstraction of your data distributed across the cluster and is resilient in nature, which means that it provides a fault-tolerant distribution and can be rebuilt if a failure happens.

+   - Distributed – The data that an RDD represents is distributed across the cluster.

You can create an RDD in two ways:

+   - Parallelize an existing collection in your driver program.

+   - Reference a dataset in an external storage system such as S3, HDFS, or HBase.

RDDs support two major types of operations:

+   - Transformations – Creation of a new dataset from an existing one, such as map, flatmap(), filter(), sample(), union(), distinct(), or groupByKey(). All transformations in Spark are lazy in nature, which means that Spark will not compute the results immediately when you call a transformation but rather wait until an action is called.

+   - Actions – Actions result in the computation of the transformations and the return of the dataset back to the driver program. Examples of transformations include reduce(), collect(), count(), first(), take(), saveAsTextFile(), and saveAsObjectFile().