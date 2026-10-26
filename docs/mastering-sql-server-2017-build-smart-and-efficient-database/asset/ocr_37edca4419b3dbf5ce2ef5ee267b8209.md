Period columns must be of the DATETIME2 data type. If you try to define them with the DATETIME data type, you will get an error. The SQL:2011 standard does not specify data type precision, thus system-versioned temporal tables in SQL Server 2017 are not implemented strictly according to standard. This is very important when you migrate your existing temporal solution to one that uses new temporal tables in SQL Server 2017. Usually, columns that you were using are of the DATETIME data type and you have to extend their data type to DATETIME2.

You can also specify the name of the history table and let SQL Server create it with the same attributes as described earlier. Use the following code to create a temporal table with a user-defined history table name:

```
USE WideWorldImporters;
CREATE TABLE dbo.Product2
(
    ProductId INT NOT NULL CONSTRAINT PK_Product2 PRIMARY KEY,
    ProductName NVARCHAR(50) NOT NULL,
    Price MONEY NOT NULL,
    ValidFrom DATETIME2 GENERATED ALWAYS AS ROW START NOT NULL,
    ValidTo DATETIME2 GENERATED ALWAYS AS ROW END NOT NULL,
    PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
)
WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.ProductHistory2));
```

What storage type is used for the automatically created history table? By default, it is a rowstore table with a clustered index on the period columns. The table is compressed with PAGE compression, if it can be enabled for compression (has no SPARSE or (B)LOB columns). To find out the storage type of the history table, use the following code:

```
SELECT temporal_type_desc, p.data_compression_desc
FROM sys.tables t
INNER JOIN sys.partitions p ON t.object_id = p.object_id
WHERE name = 'ProductHistory2';
```

The result of the preceding query shows that PAGE compression has been applied:

| temporal_type_desc | data_compression_desc |
|--------------------|-----------------------|
| HISTORY_TABLE      | PAGE                  |