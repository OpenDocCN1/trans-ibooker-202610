# How to increase DBA productivity

To implement this recipe, perform the following steps:

+   1. Build and test the T-SQL statements to retrieve the required Query Store statistics:

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

+   2. Create an additional T-SQL statement containing similar performance-related columns, filtered to a specific stored procedure that also retrieves the collection interval times:

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