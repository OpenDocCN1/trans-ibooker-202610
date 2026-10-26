

最后，第四部分是将 SYSTEM_VERSIONING 属性设置为 ON。当您执行前面的代码时，实际上是在指示 SQL Server 自动为 dbo.Product 时态表创建一个历史表。该表将创建在相同的架构（dbo）中，名称遵循以下命名约定：

```
MSSQL_TemporalHistoryFor_<current_temporal_table_object_id>_[suffix].
```

后缀是可选的，仅当表名的第一部分不唯一时才会添加。以下屏幕截图显示了当您打开 SQL Server Management Studio (SSMS) 并找到 dbo.Product 表时所看到的内容：

![](img/07d1225e179d5df9f2b2d26b026386c6_191_0.png)

SQL Server Management Studio 中的时态表

您可以看到所有时态表都有一个表示时态的小时钟图标。在表名下方，您可以看到其历史表。请注意，两个表中的列是相同的（列名、数据类型、精度、是否可为空），但历史表没有约束（主键）。

周期列必须使用 DATETIME2 数据类型。如果您尝试使用 DATETIME 数据类型定义它们，将会出现错误。SQL:2011 标准没有指定数据类型精度，因此 SQL Server 2017 中的系统版本控制时态表并非严格按标准实现。当您将现有的时态解决方案迁移到使用 SQL Server 2017 中新时态表的解决方案时，这一点非常重要。通常，您一直在使用的列是 DATETIME 数据类型，您必须将其数据类型扩展为 DATETIME2。

您还可以指定历史表的名称，并让 SQL Server 以与前面所述相同的属性创建它。使用以下代码创建具有用户定义历史表名称的时态表：

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

自动创建的历史表使用什么存储类型？默认情况下，它是一个行存储表，并在周期列上建有聚集索引。如果可以启用压缩（没有 SPARSE 或 (B)LOB 列），则该表会使用 PAGE 压缩。要查明历史表的存储类型，请使用以下代码：

```
SELECT temporal_type_desc, p.data_compression_desc
FROM sys.tables t
INNER JOIN sys.partitions p ON t.object_id = p.object_id
WHERE name = 'ProductHistory2';
```

上述查询的结果显示已应用 PAGE 压缩：

| temporal_type_desc | data_compression_desc |
|--------------------|-----------------------|
| HISTORY_TABLE      | PAGE                  |

您还可以看到历史表有一个聚集索引。以下代码提取索引名称以及索引中使用的列：

```
SELECT i.name, i.type_desc, c.name, ic.index_column_id
FROM sys.indexes i
INNER JOIN sys.index_columns ic on ic.object_id = i.object_id
INNER JOIN sys.columns c on c.object_id = i.object_id AND ic.column_id = c.column_id
WHERE OBJECT_NAME(i.object_id) = 'ProductHistory2';
```

此查询的输出显示，自动创建的历史表在周期列上有一个聚集索引，名称格式为 ix_<历史表名>：

| name | type_desc | name | index_column_id |
|-------------------|------------|----------|----------------|
| ix_ProductHistory2 | CLUSTERED | ValidFrom | 1 |
| ix_ProductHistory2 | CLUSTERED | ValidTo | 2 |

当您只想使用日期作为条件查询历史表时，这个索引是一个不错的选择。但是，当您想要浏览某个项的历史记录时，这个索引就不合适了。

如果历史表的预定义实现（行存储和聚集索引中的周期列）不符合您对历史数据的要求，您可以创建自己的历史表。当然，您需要遵守本章开头列出的所有约束和限制。以下代码首先创建一个历史表，然后创建一个时态表，最后将历史表分配给它。请注意，为了继续执行代码，您需要删除本章第一个示例中创建的时态表：

```
USE WideWorldImporters;
ALTER TABLE dbo.Product SET (SYSTEM_VERSIONING = OFF);
ALTER TABLE dbo.Product DROP PERIOD FOR SYSTEM_TIME;
DROP TABLE IF EXISTS dbo.Product;
DROP TABLE IF EXISTS dbo.ProductHistory;
GO
CREATE TABLE dbo.ProductHistory
(
    ProductId INT NOT NULL,
    ProductName NVARCHAR(50) NOT NULL,
    Price MONEY NOT NULL,
    ValidFrom DATETIME2 NOT NULL,
    ValidTo DATETIME2 NOT NULL
);
CREATE CLUSTERED COLUMNSTORE INDEX IX_ProductHistory ON dbo.ProductHistory;
CREATE NONCLUSTERED INDEX IX_ProductHistory_NC ON
dbo.ProductHistory (ProductId, ValidFrom, ValidTo);
GO
CREATE TABLE dbo.Product
(
    ProductId INT NOT NULL CONSTRAINT PK_Product PRIMARY KEY,
    ProductName NVARCHAR(50) NOT NULL,
    Price MONEY NOT NULL,
    ValidFrom DATETIME2 GENERATED ALWAYS AS ROW START NOT NULL,
    ValidTo DATETIME2 GENERATED ALWAYS AS ROW END NOT NULL,
    PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
)
WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.ProductHistory));
```

您将在本章后面更详细地了解如何更改和删除系统版本控制的表。在这里，您应该关注这样一个事实：您可以创建自己的历史表，并在其上建立聚集列存储索引。以下屏幕截图显示了当您查看 SSMS 并找到已创建的时态表时所看到的内容：

![](img/07d1225e179d5df9f2b2d26b026386c6_194_0.png)

您可以看到，由您创建的表充当历史表，并具有聚集列存储索引。

## 将周期列作为隐藏属性

周期列用于支持数据的时态性，没有业务逻辑价值。通过使用 HIDDEN 子句，您可以隐藏新的 PERIOD 列，以避免影响未设计为处理新列的现有应用程序。以下代码将创建两个时态表，一个使用默认值（可见的周期列），另一个使用隐藏的周期列：

```
CREATE TABLE dbo.T1(
    Id INT NOT NULL CONSTRAINT PK_T1 PRIMARY KEY,
    Col1 INT NOT NULL,
    ValidFrom DATETIME2 GENERATED ALWAYS AS ROW START NOT NULL,
    ValidTo DATETIME2 GENERATED ALWAYS AS ROW END NOT NULL,
    PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
)
WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.T1_Hist));
GO
INSERT INTO dbo.T1(Id, Col1) VALUES(1, 1);
GO
CREATE TABLE dbo.T2(
    Id INT NOT NULL CONSTRAINT PK_T2 PRIMARY KEY,
    Col1 INT NOT NULL,
    ValidFrom DATETIME2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL,
    ValidTo DATETIME2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL,
    PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
)
WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.T2_Hist));
GO
INSERT INTO dbo.T2(Id, Col1) VALUES(1, 1);
GO
```

当您查询两个表时，可以看到第二个表的周期列没有被列出：

```
SELECT * FROM dbo.T1;
SELECT * FROM dbo.T2;
```

结果如下：

```
Id  Col1        ValidFrom                 ValidTo
--- ------ -------------------------- --------------------------
1   1         2017-12-14 23:05:44.2068702 9999-12-31 23:59:59.9999999
```

```
Id      Col1
------  --------
1       1
```

如您所见，即使您在查询中使用 *，周期列也不会显示。因此，您可以透明地在表上实现时态功能，并且可以确保现有代码仍然有效。当然，如果您显式指定周期列的名称，它们将会显示在结果集中。

> 在此，我需要表达对另一种实现的担忧，这种实现处理的是 SELECT * 的场景。我可以理解供应商不想引入可能破坏客户现有应用程序的功能，但另一方面，当新功能和解决方案不反对不良开发习惯时，您也不能期望开发人员停止使用 SELECT *。

隐藏属性使您可以将普通表转换为时态表，而无需担心应用程序中的重大更改。

## 将非时态表转换为时态表

在 SQL Server 2017 中，您可以从头创建时态表，也可以更改现有表并为其添加属性，使其转换为系统版本控制的时态表。您需要做的就是添加周期列，在它们上定义 SYSTEM_TIME 周期，然后设置时态属性。例如，在本节中，您将把 AdventureWorks2017 数据库中的 Department 表转换为时态表。以下屏幕截图显示了该表的内容：

