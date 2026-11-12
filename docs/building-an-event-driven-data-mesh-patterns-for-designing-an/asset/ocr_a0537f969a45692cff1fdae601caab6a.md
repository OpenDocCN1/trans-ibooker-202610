Big data architecture introduced a significant shift in the mentality toward data. Not only did it address the capacity issues of existing OLAP systems, it introduced a new concept into the data world: it was not only acceptable, but preferable, to use unstructured or semi-structured data, instead of enforcing schema on write as in a data warehouse. In this new world of big data, you were free to write data with or without any schema or structure and resolve it all later at query time by applying a schema on read. Many data engineers were pleased to get rid of the schema on write requirement, as it made it easier to just get data into the ecosystem.

Consider Table 1-1, comparing the data structures and use cases between the relational database and MapReduce. MapReduce is an early Hadoop processing framework and is what you would use to read data, apply a schema (on read), perform transformations and aggregations, and produce the final result.

# Table 1-1. A comparison of a relational database management system and Hadoop, circa 2009

| | Traditional RDBMS | MapReduce |
|---|---|---|
| Data size | Gigabytes | Petabytes |
| Access | Interactive and batch | Batch |
| Updates | Read and write many times | Write once, read many times |
| Structure | Static schema | Dynamic schema |
| Integrity | High | Low |
| Scaling | Nonlinear | Linear |

Note that this definitive guide from 2009 promotes MapReduce as a solution for handling low integrity data with a dynamic schema, emphasizing the notion that HDFS should be storing unstructured data at low integrity, with varying and possibly conflicting schemas to be resolved at runtime. It also points out that this data is write once, read many times, which is precisely the scenario in which you want a strong, consistent, enforced schema—providing ample opportunity for well-meaning but unconstrained users of the data to apply a read-time schema that misinterprets or invalidates the data.

A schema on write, including well-defined columns with types, defaults, nullability, and names, doesn’t necessarily restrict downstream transformations in any way. It also doesn’t force you to denormalize or join any data before writing.

A schema does provide you with a sanity check, ensuring that the data you’re ingesting at least fits the most basic expectations of the downstream processors. Abandoning that sanity check pushes the detection of errors downstream, causing significant hardships for those trying to use the data.