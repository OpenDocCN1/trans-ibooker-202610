# Returning the difference between two table rows

Another example where you can use `OPENJSON` is to return the difference between two rows in a table. For instance, when you put application settings for different environments in a database table, you might need to know what is different in the settings between the two environments. You can accomplish this task by comparing values in each column, but this can be annoying and error prone if the table has many columns.

The following example returns the difference for database settings in the **master** and **model** database in an instance of SQL Server 2017:

```
SELECT
    mst.[key],
    mst.[value] AS mst_val,
    mdl.[value] AS mdl_val
FROM OPENJSON ((SELECT * FROM sys.databases WHERE database_id = 1 FOR JSON
AUTO, WITHOUT_ARRAY_WRAPPER)) mst
INNER JOIN OPENJSON((SELECT * FROM sys.databases WHERE database_id = 3 FOR
JSON AUTO, WITHOUT_ARRAY_WRAPPER)) mdl
ON mst.[key] = mdl.[key] AND mst.[value] <> mdl.[value]
```

Here is the list showing columns that have different values for these two databases.

| key | mst_val | mdl_val |
|---|---|---|
| name | master | model |
| database_id | 1 | 3 |
| snapshot_isolation_state | 1 | 0 |
| snapshot_isolation_state_desc | ON | OFF |
| recovery_model | 3 | 1 |
| recovery_model_desc | SIMPLE | FULL |
| is_db_chaining_on | true | false |
| target_recovery_time_in_seconds | 0 | 60 |

This is very handy and efficient; you don't need to know or write a lot of OR statements with column names. For instance, in the system view used in this example (`sys.databases`), there are 78 columns and you would need to include them all in the `WHERE` clause in a relational Transact-SQL statement.