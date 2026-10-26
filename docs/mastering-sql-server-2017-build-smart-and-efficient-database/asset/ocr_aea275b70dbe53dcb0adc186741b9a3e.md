+   - Filtered nonclustered indexes
+   - Table partitioning
+   - Indexed views
+   - Row compression
+   - Page compression
+   - Appropriate query techniques

# Joins and indexes

SQL Server executes a query by a set of physical operators. Because these operators iterate through rowsets, they are also called iterators. There are different join operators, because when performing joins, SQL Server uses different algorithms. SQL Server supports three basic algorithms: nested loops joins, merge joins, and hash joins.

The nested loops algorithm is a very simple and, in many cases, efficient algorithm. S

QL Server uses one table for the outer loop, typically the table with the fewest rows. For each row in this outer input, SQL Server seeks matching rows in the second table, which is the inner table. SQL Server uses the join condition to find the matching rows. The join can be a non-equijoin, meaning that the equality operator does not need to be part of the join predicate. If the inner table has no supporting index to perform seeks, then SQL Server scans the inner input for each row of the outer input. This is not an efficient scenario. A nested loop join is efficient when SQL Server can perform an index seek in the inner input.

Merge join is a very efficient join algorithm. However, it has its own limitations. It needs at least one equijoin predicate and sorted inputs from both sides. This means that the merge join should be supported by indexes on both tables involved in the join. In addition, if one input is much smaller than another, then the nested loop join could be more efficient than a merge join.