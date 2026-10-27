
(MB)] +
[User Hierarchy Size (MB)] + [Dictionary Size (MB)] + [Data Size (MB)]

```
Last Refresh Message = VAR RefreshDate = MAX('Columns'[Date Retrieved])
RETURN "Last Refreshed: " & RefreshDate
```

+   7. Create a Power BI report page based on the retrieved and modeled DMV data, as seen in Figure 10.5:

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_469_0.png)

Figure 10.5: Summary Memory report of the Power BI dataset

## How it works...

For the Segments query, the Server and Database parameters, along with a SQL statement against the DISCOVER_STORAGE_TABLE_COLUMN_SEGMENTS DMV, are used to extract and access memory usage data from the running Power BI Desktop file (which contains an analysis services instance). The conditional column, Structure Type, is added to identify the memory structure represented by each row, and a concatenated column, ColumnKey, is created to support a relationship with the Columns table.

For the Columns query, the DISCOVER_STORAGE_TABLE_COLUMNS DMV is filtered based on the Column_Type column and a concatenated column, ColumnKey, is created for the relationship with the Segments table. Additionally, a dynamic Date Retrieved column is added to support the reporting layer.

The memory columns for both source DMVs are in terms of bytes. Megabytes (MB) are more intuitive and presentable given that they have fewer digits; thus, the BytesPerMB measure (which is hardcoded to 1,048,576) is used to DIVIDE the SUM of the USER_SIZE and DICTIONARY_SIZE columns. The Structure Type column created in the Segments query is used to support different components of overall memory usage, and two total measures are created for summary-level reports.

Card visuals are used to display overall memory usage; two table visuals with top N visual-level filters provide additional details. In this example, the **Reseller Sales XL** table is by far the largest table in the model, and specifically, the **CarrierTrackingNumber**, **SalesOrderNumber**, and **CustomerPONumber** columns of this table are consuming the most memory. Clicking on a table name in the **Top 5 Tables** visual cross-filters the **Top 5 Columns** visual.

More DMVs and logic can be added to build a more robust template report for use across Power BI projects. Note that the **Server** and **Database** parameters will need to be updated when a PBIX file being analyzed has been closed and reopened.

## There's more...

In addition to the compressed data memory structures for each column, dictionary and column hierarchy structures (**H$**) are also created internally for imported data models to support queries. The dictionary structure, retrieved via the columns DMV, stores the unique values of a column, and so it is larger for high-cardinality columns. Two additional memory structures include user-defined hierarchies (**U$**) and table relationships (**R$**).

In this recipe, the memory allocated to column hierarchy structures (**H$**) is excluded from the **Total Column Size** measure but included in the **Total Size** measure. This is intended to focus analysis on the larger components of memory that can be directly impacted by revisions to the data model. Likewise, the memory used for relationships and user-defined hierarchies is small relative to the data and dictionary size of the model's columns.

Basic performance tuning of imported data models largely focuses on minimizing high-cardinality columns and relationships to reduce the memory scanned. More advanced tuning, generally reserved for SSAS models, involves the partitioning of tables, segment sizes, and optimizing the sort order used by the engine during compression. Only the sort order is available to Power BI datasets.

The Vertipaq Analyzer utilizes DMVs to collect information on datasets and is now integrated into DAX Studio under the **Advanced** menu as well as in Tabular Editor 3. Using the Vertipaq Analyzer is a quicker and easier way to analyze the memory size, cardinality, and other aspects of your data model versus writing your own queries against the DMVs.

## See also

- Kasper de Jonge has published a blog on building an SSAS memory report with Power BI: http://bit.ly/2tDumgk.
- SQL BI's Vertipaq Analyzer Excel file: http://bit.ly/2sTTuS0.
- **Dynamic Management Views (DMVs)** in Analysis Services: https://bit.ly/33etDoc.

Vertipaq Analyzer script for Tabular Editor 2: https://bit.ly/2YA97zv

## Increasing DBA productivity

SQL Server Query Store is a monitoring feature available to all editions of SQL Server 2016 and later—it significantly simplifies and expedites query tuning and troubleshooting. The Query Store database provides aggregated metrics regarding query executions, query plans, and wait statistics to enable visibility to performance trends and usage patterns.

> "Query Store is a fantastic flight data recorder for your execution plans. It will help you troubleshoot parameter sniffing issues, connection settings issues, plan regressions, bad stats, and much more."

—Brent Ozar, Author and Microsoft-Certified Master in SQL Server

Query Store includes a graphical interface of charts and user controls, and its schema lends itself to custom T-SQL queries such as 10 longest-running queries in the past hour. While these are great features and sufficient for certain scenarios, administrators often have to make trade-offs between the flexibility of T-SQL and the graphical controls provided by Query Store. In this recipe, simple T-SQL statements are passed from Power BI to SQL Server Query Store in order to identify and analyze recent performance issues, as well as the performance of a specific stored procedure.

## Getting ready

To prepare for this recipe, follow these steps:

- 1. Enable Query Store in the latest version of SQL Server Management Studio (SSMS), either via the Database Properties dialog in the Object Explorer interface (right-click on the database and choose Properties) or via the following T-SQL statement:

```
ALTER DATABASE AdventureWorksDW2019 SET QUERY_STORE = ON;
```

- 2. Configure Query Store settings, such as Statistics Collection Intervals, Retention, and Max Size (MB) according to your requirements.

In this example, the performance of individual queries will be aggregated or summarized into 5-minute time frames. Smaller collection time intervals provide greater details, but also require more storage and collection resources. For maximum detail with no grouping of queries, an Extended Events session can be scheduled. Refer to the Analyzing Extended Events recipe later in this chapter for more details.

## How to increase DBA productivity

To implement this recipe, perform the following steps:

- 1. Build and test the T-SQL statements to retrieve the required Query Store statistics:

```
SELECT
    [rs].[avg_duration], [rs].[avg_logical_io_reads], [qst].[query_text_id],
    [qsq].[query_id],
    [qst].[query_sql_text], CASE WHEN [qsq].[object_id] = 0
        THEN N'Ad-hoc' ELSE OBJECT_NAME([qsq].[object_id])
        END AS [ObjectName],
    [qsp].[plan_id], GETUTCDATE() AS CurrentUTCtime,
    [rs].[last_execution_time],
        CAST((DATEADD(MINUTE,
            -(DATEDIFF(MINUTE, GETDATE(), GETUTCDATE())),
            [rs].[last_execution_time])) AS datetime2(0))
        AS LocalLastExecutionTime
FROM
    [sys].[query_store_query] [qsq]
    JOIN [sys].[query_store_query_text] [qst] ON
        [qsq].[query_text_id] = [qst].[query_text_id]
    JOIN [sys].[query_store_plan] [qsp] ON
        [qsq].[query_id] = [qsp].[query_id]
    JOIN [sys].[query_store_runtime_stats] [rs] ON
        [qsp].[plan_id] = [rs].[plan_id]
WHERE [rs].[last_execution_time] > DATEADD(hour, -8, GETUTCDATE())
```

- 2. Create an additional T-SQL statement containing similar performance-related columns, filtered to a specific stored procedure that also retrieves the collection interval times:

```
SELECT * FROM
[sys].[query_store_query] [qsq]
JOIN [sys].[query_store_query_text] [qst] ON
    [qsq].[query_text_id] = [qst].[query_text_id]
JOIN [sys].[query_store_plan] [qsp] ON
    [qsq].[query_id] = [qsp].[query_id]
JOIN [sys].[query_store_runtime_stats] [rs] ON
    [qsp].[plan_id] = [rs].[plan_id]
JOIN [sys].[query_store_runtime_stats_interval] [rsi] ON
    [rs].[runtime_stats_interval_id] = [rsi].[runtime_stats_interval_id]
WHERE [qsq].[object_id] = OBJECT_ID(N'Sales.usp_GetFullProductInfo')
```
