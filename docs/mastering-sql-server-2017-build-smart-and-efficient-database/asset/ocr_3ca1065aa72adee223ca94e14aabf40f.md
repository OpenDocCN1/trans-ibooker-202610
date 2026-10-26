Now you can issue the following three `ALTER` statements that correspond to the previously described new columns:

```
ALTER TABLE dbo.Product ADD Color NVARCHAR(15);
ALTER TABLE dbo.Product ADD Category SMALLINT NOT NULL CONSTRAINT DF_Category DEFAULT 1;
ALTER TABLE dbo.Product ADD Description NVARCHAR(MAX) NOT NULL CONSTRAINT DF_Description DEFAULT N'N/A';
```

The content collected by the XE session is shown as follows:

![](bbox=[0.105, 0.292, 0.896, 0.481])

It clearly shows that the first two `ALTER` statements have been executed without additional actions. However, adding a LOB column triggers the updating of all rows with the default value as an offline operation.

The first statement adds a column that accepts null values, so the action is done instantly.

When you add a non-nullable column, the situation is different, as illustrated in the second statement. First, you need to provide a default constraint to ensure that all rows will have a value in this column. This action will be online (metadata operation) in the Enterprise and Developer editions only. In all the other editions, all rows in both the current and the history table will be updated to add additional columns with their values.

However, adding LOB or BLOB columns will cause a mass update in both the current and the history table in all SQL Server editions! This action will internally update all rows in both tables. For large tables, this can take a long time and during this time, both tables are locked.