If you need to perform schema changes to a temporal table not supported in the `ALTER` statement, you have to set its `SYSTEM_VERSIONING` attribute to false to convert the tables to non-temporal tables, perform the changes, and then convert back to a temporal table. The following code demonstrates how to add the identity column `ProductNumber` and the sparse column `Size` into the temporal table `dbo.Product`:

```
```sql
ALTER TABLE dbo.ProductHistory REBUILD PARTITION = ALL WITH
(DATA_COMPRESSION=NONE);
GO
BEGIN TRAN
ALTER TABLE dbo.Product SET (SYSTEM_VERSIONING = OFF);
ALTER TABLE dbo.Product ADD Size NVARCHAR(5) SPARSE;
ALTER TABLE dbo.ProductHistory ADD Size NVARCHAR(5) SPARSE;
ALTER TABLE dbo.Product ADD ProductNumber INT IDENTITY (1,1);
ALTER TABLE dbo.ProductHistory ADD ProductNumber INT NOT NULL DEFAULT 0;
ALTER TABLE dbo.Product SET(SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.
ProductHistory));
COMMIT;
```
```

To perform `ALTER TABLE` operations, you need to have `CONTROL` permission on the current and history tables. During the changes, both tables are locked with schema locks.

# Dropping temporal tables

You cannot drop a system-versioned temporal table. Both current and history tables are protected until the `SYSTEM_VERSIONING` attribute of the current table is set to `ON`. When you set it to `OFF`, both tables automatically become non-temporal tables and are fully independent of each other. Therefore, you can perform all operations against them that are allowed according to your permissions. You can also drop the period if you definitely want to convert a temporal table to a non-temporal one. The following code converts the `Product` table into a non-temporal table and removes the defined `SYSTEM_TIME` period:

```
```sql
ALTER TABLE dbo.Product SET (SYSTEM_VERSIONING = OFF);
ALTER TABLE dbo.Product DROP PERIOD FOR SYSTEM_TIME);
```
```

Note that the period columns `ValidFrom` and `ValidTo` remain in the table and will be further updated by the system. However, the history table will not be updated when data in the current table is changed.