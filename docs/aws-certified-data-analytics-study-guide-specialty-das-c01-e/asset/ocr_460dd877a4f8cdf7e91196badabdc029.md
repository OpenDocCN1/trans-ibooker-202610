# FIGURE 4.21 Relationship between streams, segments, and steps

![](bbox=[0.3, 0.13, 0.7, 0.42])

# Explain Plans in Redshift

You can generate explain plans using the EXPLAIN statement, which allows you to generate a pre-formatted output with one row per step and a single column indicating the step. If you use Redshift Query Editor, you can generate a visual explain plan. Explain plans are read from the bottom up, and they provide information like the following:

+   - Execution order of steps for local and Lakehouse portions
+   - Estimated number of rows to be returned by a step
+   - Amount of data to be sent across slices per step (Network I/O is expensive and should be optimized as much as possible.)
+   - Relative cost of a step

You can look at historic trends of explain plans generated for queries that were previously run on the system using STL_EXPLAIN table. One of the key components in understanding explain plans is understanding the terminology being used during the explain plan generation. As discussed earlier, Redshift is an MPP database, where many compute nodes work together to produce efficient results, which means that the network becomes a really important component during data processing, especially join processing. During a particular step in a join, you may need to work on data that is not stored locally and hence data from a different slice on the same node, or in the worst-case scenario, a different node may need to be fetched. Since network transmission is by far the most expensive operation a query will do, it is important to understand how data is moving across the nodes. Table 4.15 describes the terminology that you will see in explain plans.