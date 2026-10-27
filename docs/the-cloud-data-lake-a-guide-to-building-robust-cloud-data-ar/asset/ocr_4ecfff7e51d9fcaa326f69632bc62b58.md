Apache Spark provides configurable parameters that can be tuned so that the job is executed with optimal resource utilization in terms of CPU and memory. This process of setting the right parameters for effective utilization of CPU and memory is called *performance tuning*. The tunable parameters of Apache Spark fall into the following categories:

# Data serialization

Apache Spark offers libraries that serialize the datasets—that is, convert logical objects into a series of bytes to be sent over the network or persist in disk storage in an efficient fashion. When you write your core Spark applications, you write code to transform the data via filtering, aggregation, joins, and so on. This dataset, whether it is the input or the transformed dataset, is serialized before it is sent to the executors, as shown in Figure 5-12. Apache Spark offers serialization in Java as well as in Kryo libraries, which offer faster, more efficient serialization compared to Java. Leveraging the Apache Spark configuration to use the Kryo serializer will provide performance optimizations, especially for networking-intensive applications, where large data transfers are going over the network with more complex transformations or using cloud data lake storage to persist the datasets. You can read more about the Kryo serializer in the Apache Spark Performance Tuning—Data serialization documentation.

![](bbox=[0.17, 0.47, 0.83, 0.77])

Figure 5-12. Datasets and executors in Spark application