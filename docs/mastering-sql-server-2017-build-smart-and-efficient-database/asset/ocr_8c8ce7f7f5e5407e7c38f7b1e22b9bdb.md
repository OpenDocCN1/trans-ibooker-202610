+   - `CloseLoadExecution`: This is one of the most complex procedures. It receives a single parameter, `@LoadExecutionId` and does the following:
+     - It retrieves the status for the `SSIExecutionId` column value for the `@LoadExecutionId` value
+     - It retrieves the row counts of various packages components for the `SSIExecutionId` and inserts them into the `ExecutionStatistics` table
+     - It assigns the `@ExecutionEndDatetime` column to system date and time
+   - `LogExecutionMessage`: This procedure receives three parameters: `@LoadExecutionId`, `@ExecutionMessageType`, and `@ExecutionMessage`. It simply inserts a row into the `SystemLog.ExecutionMessages` table.
+   - `LogExecutionRejects`: This procedure is used to log rows that have been rejected by the package executions. It has three parameters: `@LoadExecutionId`, `@RejectCode`, and `@RowValues`.

This constitutes the basics of an ETL framework. As well see in future recipes, we'll use the preceding procedures in the entry-point packages.

The `Loads`, `ExecutionStatus`, and `ExecutionMessageTypes` tables have some predefined data inserted via a `ScriptPostDeployment`.

Now we're going to insert the framework procedure calls into the `EP_Staging` package.

# Getting ready

This recipe assumes that you have access to this book's companion solution.