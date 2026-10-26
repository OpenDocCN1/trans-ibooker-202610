However, when you look at the execution plans (see the following screenshot) for the execution of the first query, both tables have been processed, while the non-temporal query had to retrieve data from the current table only:

![](bbox=[0.1, 0.2, 0.9, 0.46])

Comparing execution plans for temporal and non-temporal queries that retrieve actual data only

So, you should not use temporal queries with the FOR SYSTEM_TIME AS clause to return data from the current table.

# Retrieving temporal data from a specific period

You can use the new FOR SYSTEM_TIME clause to retrieve temporal data that was or is valid between two points in time. These queries are typically used for getting changes to specific rows over time. To achieve this, you could use one of two SQL:2011 standard-specified sub-clauses:

+   - FROM...TO returns all data that started before or at the beginning of a given period and ended after the end of the period (closed-open interval)
+   - BETWEEN...AND returns all data that started before or at the beginning of a given period and ended after or at the end of the period (closed-closed interval)