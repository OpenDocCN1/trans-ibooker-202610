If there are required transformations or logic that are not supported by the source system for query folding, the recommendation is to move these steps to the very end of the query. For example, allow SQL Server to execute the filter, the derived columns, and other simple steps via query folding, and only then apply the complex steps locally on top of the SQL query result set.

Several common M functions are not supported by most relational database sources, such as `Table.Distinct`, which removes duplicate rows from tables, and `Table.RemoveRowsWithErrors`, which removes rows with errors from tables. If data sources are merged in the query, check their privacy settings (**Data source settings | Edit Permissions...**) to ensure that privacy is configured to allow folding, such as from an organizational source to a different organizational source.

In general, the following operations are not supported by query folding:

+   - Changing column data types
+   - Adding index columns
+   - Merging or appending queries from different sources
+   - Adding custom columns using Power Query functions that have no equivalent function in the data source

# There's more...

For large models with many queries and large tables, consider disabling the default parallel loading of tables in **File | Options | Options and settings | CURRENT FILE** as many queries executed simultaneously may overwhelm source system resources and cause the refresh process to fail.

![](bbox=[0.133, 0.593, 0.867, 0.681])

Figure 11.12: Parallel loading of tables – CURRENT FILE setting

Just because a query is folded into a SQL statement, it does not mean there are no possible performance issues. For example, the query might be selecting more columns than needed by the data model or might be executing outer join queries when the database schema supports inner joins. Visibility of these queries can inform changes to the BI architecture and M queries.

Owners of the relational database system or data warehouse can take note of Power BI's folded SQL queries via tools like **Extended Events**. For example, database administrators or BI team members could revise existing SQL views, table indexes, and more based upon the information gathered. Likewise, the Power BI query author could be informed of better or preferred methods of accessing the same data such as joining on different columns.