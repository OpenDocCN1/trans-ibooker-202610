
You need to ensure that you can write queries that reference both dbo.ClientCases and dbo.Clients in the same SQL query, without making additional copies of the tables.
What should you use?

+   a. A shortcut
+   b. A view
+   c. A dataflow
+   d. A managed table

7. You’re working with Dataflow Gen2 to implement a solution designed to make sure the system ignores the spaces in the values when performing a join between two tables.
What should you do?

+   a. Use a lookup table.
+   b. Use fuzzy matching.
+   c. Use fuzzy grouping.
+   d. Use a reference table.

8. You’re working in Dataflow Gen2, and you want to merge two queries into one. You need to ensure that the query returns all the rows that are present in both tables. Which type of join should you use?

+   a. LEFT OUTER JOIN
+   b. LEFT ANTI JOIN
+   c. RIGHT OUTER JOIN
+   d. FULL OUTER JOIN

## e. INNER JOIN

9. You have a Fabric workspace that’s allocated to an F-128 capacity. The workspace contains a KQL database with a table named DeviceLogs.

DeviceLogs contains 25 columns named c1 through c25 and 10 billion rows. Column c1 contains identifiers ranging from 1 to 15 that are equally distributed.

You have the following query:

```
DeviceLogs
| project c1, c2, c3
| where c1 == 1
```

You modify the query as follows:

```
DeviceLogs
| where c1 == 1
| project c1, c2, c3
```

How will the execution time of the query be impacted?

+   a. The execution time will not change.
+   b. The execution time will be shorter.
+   c. The query will fail.
+   d. The execution time will be longer.

10. You have a semantic model named Model1. Model1 contains a single table named Orders with the following columns:

Order Date

Product Name

Quantity

You have a report that contains a page named Orders, which contains a slicer for the Product Name field and a clustered column chart that shows Quantity by Order Date.

You need to allow users the option to choose between Quantity by Product Name and Quantity by Order Date in the column chart. The solution must minimize development effort.

What should you do?

+   a. Create a measure.
+   b. Create a field parameter.
+   c. Create a dynamic format string.
+   d. Create a calculation group.

11. You have a Fabric workspace with a warehouse named Warehouse1. You have a user named User1 who has the viewer role for the same workspace. You need to delegate permissions to User1 to meet the following requirements:

+   - User1 must be able to share the warehouse to other users.
+   - The solution must follow the principle of least privilege.

Which permission should you assign to User1?

+   a. The member role in the workspace
+   b. The admin role in the workspace
+   c. The contributor role in the workspace
+   d. The db_owner role in the warehouse

12. You have a Fabric workspace named Workspace1 that contains a lakehouse and includes a user named User1.

You need to ensure that User1 can read all the data in the lakehouse but not any other items or data from the workspace. Which two actions should you perform?

+   a. Share the lakehouse by using item permissions.
+   b. Select “Read all Apache Spark data.”
+   c. Add User1 to the viewer role.
+   d. Add User1 to the contributor role.
+   e. Add User1 to the member role.

13. Your task is to connect a Fabric workspace to a GitHub repository. What information do you need to accomplish this?

+   a. A PAT and the URL for the repository
+   b. A SAS token and the branch name for the repository
+   c. An access key and the branch name for the repository
+   d. An access key and the URL for the repository

14. You have a workspace in Fabric with a warehouse and a lakehouse.

The warehouse contains a table named dbo.FactSalesSmall, and the lakehouse contains a Delta table named dbo.FactSalesConsolidated.

Your task is to copy the data from dbo.FactSalesSmall to dbo.FactSales Consolidated, using a low-code solution.

What should you do?

+   a. Use a notebook to read the data from Table1 and insert the data into Table2.
+   b. Use a Dataflow Gen2 to read the data from FactSalesSmall and insert the data into FactSalesConsolidated.
+   c. Use a stored procedure to read the data from FactSalesConsolidated and insert the data into FactSalesSmall.
+   d. Use a notebook to read the data from FactSalesConsolidated and insert the data into FactSalesSmall.

15. In a Fabric workspace, you have a Dataflow Gen2 with complex business logic implemented. You need to persist the output to a lakehouse in the workspace. What two options do you have to configure the destination?

+   a. Configure the default destination.
+   b. Configure the destination for the current flow.
+   c. Configure the destination as a new step in the flow pane.
+   d. Configure the destination using a notebook.
+   e. Configure the destination using a pipeline.

16. You’re working in Microsoft Fabric, and you have a workspace with several reports and a semantic model that contains information about a future acquisition to be made.

You need to make sure that data from the items does not fall into the wrong hands, and the solution must ensure that future items in the workspace inherit the settings. What should you do?

+   a. Plan a configuration of tags.
+   b. Plan a configuration of sensitivity labels.
+   c. Plan a configuration of endorsement.
+   d. Split the report and semantic model into two workspaces.

17. An analytics engineer is designing a new data solution in Microsoft Fabric. The solution will involve a central data store that is frequently accessed for T-SQL and Spark queries. The data engineers need to be able to access semi-structured and unstructured data, and the data store must also support row-level security for users executing T-SQL queries. Which type of data store is the most appropriate choice for this solution?

+   a. A data lake
+   b. An external Hive metastore
+   c. A warehouse
+   d. A lakehouse

18. You’re creating a report visual in Power BI that displays a customer’s average satisfaction score based on surveys submitted over the past 12 months. The data needs to update as soon as the source data changes and be as fast as possible.
Which storage mode should you configure for the semantic model to meet this requirement?

+   a. DirectQuery
+   b. Direct Lake
+   c. Import
+   d. Live Connect

19. An analytics team needs to implement row-level security (RLS) on a table in a semantic model to restrict data visibility based on the user accessing the report.
Which of the following DAX functions is most often used to define RLS rules?

+   a. FILTER
+   b. LOOKUPVALUE
+   c. USERPRINCIPALNAME
+   d. CALCULATE

20. You have a data warehouse in Microsoft Fabric that contains a table named Stage.Customers, which receives incremental updates from a CRM system. You need to write a T-SQL query that returns the single most recent update for each customer. Which T-SQL function should you use to achieve this efficiently?

+   a. ROW_NUMBER
+   b. RANK
+   c. DENSE_RANK
+   d. GROUP BY

21. A data analyst needs to create and share semantic models with other data analysts within a Fabric workspace. The analyst needs to have full control over the models and reports and must use the least-privilege approach.

Which of the following workspace roles would provide the necessary permissions?

+   a. Administrator
+   b. Member
+   c. Viewer
+   d. Contributor

22. Which of the following describes the purpose of a calculation group in a semantic model?

+   a. To perform row-level security (RLS) by grouping users into roles
+   b. To create a composite model by grouping tables from different sources
+   c. To group multiple measures and apply a common calculation modifier to them
+   d. To create new columns in a table based on a single DAX expression

23. A data engineer has a table in a lakehouse that contains sales data, but some of the records have NULL values in the CustomerID column. What is the most effective and low-code way to identify and resolve these missing values using the data transformation capabilities in Microsoft Fabric?

+   a. Create a T-SQL view that filters out the NULL values.
+   b. Use a Python notebook to fill the NULL values with a default value.
+   c. Configure incremental refresh on the semantic model to ignore the bad data.
+   d. Use the data profiling tools in a Dataflow Gen2 to check column quality and fill in missing values.

24. Which of the following is the primary purpose of a deployment pipeline in Microsoft Fabric?

+   a. To monitor the performance of report visuals and queries to help optimize the performance of the platform
+   b. To schedule a data refresh for a semantic model to keep the data as fresh and up-to-date as possible
+   c. To manage the continuous integration and continuous deployment (CI/CD) of analytics solutions
+   d. To create a copy of a workspace for backup purposes in case of a need for disaster recovery
