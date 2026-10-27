# Accessing and Retrieving Data

Finding and revising queries that are not being folded to source systems is a top technique for enhancing large Power BI datasets. See the *Pushing Query Processing Back to Source Systems* recipe of Chapter 11, *Enhancing and Optimizing Existing Power BI Solutions*, for an example of this process.

The M query engine also supports partial query folding. A query can be "partially folded", in which a SQL statement is created resolving only part of an overall query. The results of this SQL statement would be returned to Power BI Desktop (or the on-premises data gateway) and the remaining logic would be computed using M's in-memory engine with local resources.

M queries can be designed to maximize the use of the source system resources, by using standard expressions supported by query folding early in the query process. Minimizing the use of local or on-premises data gateway resources is a top consideration for improving query performance.

There are limits, however, to query folding. For example, no folding takes place once a native SQL query has been passed to the source system, such as when passing a SQL query directly through the **Get Data** dialog using the **Advanced options**. *Figure 2.10* displays a query specified in the **Get Data** dialog, which is included in the **Source** step.

![](bbox=[0.132, 0.403, 0.868, 0.814])

Figure 2.10: Providing a user-defined native SQL query