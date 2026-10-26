+   - **Partition switching**: This is a process that switches a block of data from one table or partition to another table or partition. You switch the data by using the `ALTER TABLE T-SQL` command. You can perform the following types of switching:
+     - Reassign all data from a nonpartitioned table to an empty existing partition of a partitioned table
+     - Switch a partition of a one-partitioned table to a partition of another partitioned table
+     - Reassign all data from a partition of a partitioned table to an existing empty nonpartitioned table
+   - **Partition elimination**: This is a query optimizer process in which SQL Server accesses only those partitions needed to satisfy query filters.

For more information about table and index partitioning, refer to the MSDN *Partitioned Tables and Indexes* article at https://msdn.microsoft.com/en-us/library/ms190787.aspx.

# Nonclustered indexes in analytical scenarios

As mentioned, DW queries typically involve large scans of data and aggregation. Very selective seeks are not common for reports from a DW. Therefore, nonclustered indexes generally don’t help DW queries much. However, this does not mean that you shouldn’t create any nonclustered indexes in your DW.

An attribute of a dimension is not a good candidate for a nonclustered index key. Attributes are used for pivoting and typically contain only a few distinct values. Therefore, queries that filter based on attribute values are usually not very selective. Nonclustered indexes on dimension attributes are not good practice.

DW reports can be parameterized. For example, a DW report could show sales for all customers or for only a single customer, based perhaps on parameter selection by an end user. For a single-customer report, the user would choose the customer by selecting that customer’s name. Customer names are selective, meaning that you retrieve only a small number of rows when you filter by customer name. Company names, for example, are typically unique, so by filtering by a company name, you typically retrieve a single row. For reports like these, having a nonclustered index on a name column or columns could lead to better performance.