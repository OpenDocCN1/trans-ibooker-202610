
Since the existing temporal data implementation will be replaced by an SQL Server 2017 temporal table, the `ModifiedDate` column is not necessary anymore and can be removed. Furthermore, you have to use two ALTER statements; with the first statement, you define the period columns and the period, while the second statement sets the `SYSTEM_VERSIONING` attribute to ON and defines the name of the history table that will be created by the system.

You should be aware that you need to provide default constraints for both columns, since they must be non-nullable. You can even use a date value from the past for the default constraint of the first period column (as shown in the preceding code example, where the value is set to the one from the removed column); however, you cannot set values in the future. Finally, period date columns are defined with the `HIDDEN` attribute to ensure that there will be no breaking changes in the existing code.

![](img/07d1225e179d5df9f2b2d26b026386c6_198_0.png)

Adding a non-nullable column with a default constraint is a metadata operation in Enterprise Edition only; in all other editions that means a physical operation with the allocation space to update all table rows with newly added columns. For large tables, this can take a long time, and be aware that, during this action, the table is locked.

```
UPDATE HumanResources.Department SET Name='Political Correctness' WHERE DepartmentID = 2;
SELECT * FROM HumanResources.Department WHERE DepartmentID = 2;
SELECT * FROM HumanResources.DepartmentHistory;
```

The first table simply shows the new value, while the second shows the previous one, with appropriate validity dates from a system point of view (the time part of the date is shortened for brevity):

| DepartmentID | Name                  | GroupName                   |
|---|---|---|
| 2 | Political Correctness | Research and Development |

| DepartmentID | Name       | GroupName                 | ValidFrom  | ValidTo   |
|---|---|---|---|---|
| 2 | Tool Design | Research and Development | 2008-04-30 | 2017-12-15 |

As you can see, it is very easy and straightforward to add temporal functionality to an existing non-temporal table. It works transparently; all your queries and commands will work without changes.

## Migrating an existing temporal solution to system-versioned tables

Most probably, you have had to deal with historical data in the past. Since there was no out-of-the-box feature in previous SQL Server versions, you had to create a custom temporal data solution. Now that the feature is available, you might think to use it for your existing temporal solutions. You saw earlier in this chapter that you can define your own history table. Therefore, you can also use an existing and populated historical table. If you want to convert your existing solution to use system-versioned temporal tables in SQL Server 2017, you have to prepare both tables so that they fill all requirements for temporal tables. To achieve this, you will again use the AdventureWorks2017 database and create both current and history tables by using tables that exist in this database. Use the following code to create and populate the tables:

```
USE WideWorldImporters;
CREATE TABLE dbo.ProductListPrice
(
    ProductID INT NOT NULL CONSTRAINT PK_ProductListPrice PRIMARY KEY,
    ListPrice MONEY NOT NULL,
);
INSERT INTO dbo.ProductListPrice(ProductID,ListPrice)
SELECT ProductID,ListPrice FROM AdventureWorks2017.Production.Product;
GO
CREATE TABLE dbo.ProductListPriceHistory
(
    ProductID INT NOT NULL,
    ListPrice MONEY NOT NULL,
    StartDate DATETIME NOT NULL,
    EndDate DATETIME NULL,
    CONSTRAINT PK_ProductListPriceHistory PRIMARY KEY CLUSTERED
    (
        ProductID ASC,
        StartDate ASC
    )
);
INSERT INTO
dbo.ProductListPriceHistory(ProductID,ListPrice,StartDate,EndDate)
SELECT ProductID, ListPrice, StartDate, EndDate FROM
AdventureWorks2017.Production.ProductListPriceHistory;
```

Consider the rows for the product with an ID of 707 in both tables:

```
SELECT * FROM dbo.ProductListPrice WHERE ProductID = 707;
SELECT * FROM dbo.ProductListPriceHistory WHERE ProductID = 707;
```

Here are the rows in the current and history tables respectively:

| ProductID | ListPrice |
|-----------|-----------|
| 707       | 34.99     |

| ProductID | ListPrice | StartDate | EndDate |
|-----------|-----------|-----------|---------|
| 707       | 33.6442   | 2011-05-31 00:00:00.000 | 2012-05-29 00:00:00.000 |
| 707       | 33.6442   | 2012-05-30 00:00:00.000 | 2013-05-29 00:00:00.000 |
| 707       | 34.99     | 2013-05-30 00:00:00.000 | NULL |

Assume that this data has been produced by your temporal data solution and that you want to use system-versioned temporal tables in SQL Server 2017 instead of it, but also use the same tables. The first thing you have to do is align the columns in both tables. Since the current table has no date columns, you need to add two period columns and define the period. The columns should have the same name as the counterpart columns from the history table. Here is the code that creates the temporal infrastructure in the current table:

```
ALTER TABLE dbo.ProductListPrice
ADD StartDate DATETIME2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL
CONSTRAINT DF_StartDate1 DEFAULT '20170101 00:00:00.0000000',
    EndDate DATETIME2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL CONSTRAINT DF_EndDate1 DEFAULT '99991231 23:59:59.9999999',
    PERIOD FOR SYSTEM_TIME (StartDate, EndDate);
GO
```

The next steps are related to the history table. As you can see from the sample data, your current solution allows gaps in the history table and also contains the current value with the undefined end date. As mentioned earlier in this chapter, the history table only contains historical data and there are no gaps between historical entries (the new start date is equal to the previous end date). Here are the steps you have to implement in order to prepare the dbo.ProductListPriceHistory table to act as a history table in a system-versioned temporal table in SQL Server 2017:

- Update the non-nullable EndDate column to remove the gap between historical values described earlier and to support the open-closed interval
- Update all rows where the EndDate column is null to the StartDate of the rows in the current table
- Remove the primary key constraint
- Change the data type for both date columns StartDate and EndDate to DATETIME2

Here is the code that implements all these requests:

```
--remove gaps
UPDATE dbo.ProductListPriceHistory SET EndDate = DATEADD(day,1,EndDate);
--update EndDate to StartDate of the actual record
UPDATE dbo.ProductListPriceHistory SET EndDate = (SELECT MAX(StartDate) FROM dbo.ProductListPrice) WHERE EndDate IS NULL;
--remove constraints
ALTER TABLE dbo.ProductListPriceHistory DROP CONSTRAINT PK_ProductListPriceHistory;
--change data type to DATETIME2
ALTER TABLE dbo.ProductListPriceHistory ALTER COLUMN StartDate DATETIME2 NOT NULL;
ALTER TABLE dbo.ProductListPriceHistory ALTER COLUMN EndDate DATETIME2 NOT NULL;
```

Now, both tables are ready to act as a system-versioned temporal table in SQL Server 2017:

```
ALTER TABLE dbo.ProductListPrice SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.ProductListPriceHistory, DATA_CONSISTENCY_CHECK = ON));
```

The command has been executed successfully and the dbo.ProductListPriceHistory table is now a system-versioned temporal table. Note that the DATA_CONSISTENCY_CHECK = ON option is used to check that all rows in the history table are valid from a temporal data point of view (no gaps and the end date is not before the start date). Now, you can check the new functionality by using the UPDATE statement. You will update the price for the product by changing the ID of 707 to 50 and then check the rows in both tables:

```
UPDATE dbo.ProductListPrice SET ListPrice = 50 WHERE ProductID = 707;
SELECT * FROM dbo.ProductListPrice WHERE ProductID = 707;
SELECT * FROM dbo.ProductListPriceHistory WHERE ProductID = 707;
```
