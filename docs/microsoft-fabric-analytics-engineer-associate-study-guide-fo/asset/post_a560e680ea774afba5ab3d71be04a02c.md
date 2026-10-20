
25. A data engineer is designing a data model for a new analytics solution in Microsoft Fabric. The model will be used by various reports and must be easy to navigate and understand for business users.
Which schema design is the most appropriate for this goal?

+   a. A snowflake schema
+   b. A flat schema
+   c. A star schema
+   d. A normalized schema

26. You’re creating a DAX measure that needs to calculate the total sales for the previous year, based on the user’s selected date. Which DAX function is the most efficient and recommended for performing this time intelligence calculation?

+   a. CALCULATE
+   b. TOTALYTD
+   c. PREVIOUSYEAR
+   d. DATEADD

27. A data analyst is building a Power BI report using a semantic model in Microsoft Fabric. They want to create a measure that calculates the total sales for the same period in the previous year. The measure must be able to respect any time filters the user applies to the report. Which of the following DAX functions should the data analyst use for this purpose?

+   a. CALCULATE(SUM('Sales'[SalesAmount]), SAMEPERIODLASTYEAR('Date'[Date]))
+   b. SAMEPERIODLASTYEAR(SUM('Sales'[SalesAmount]))
+   c. CALCULATE(SUM('Sales'[SalesAmount]), DATESYTD(DATEADD('Date'[Date], -1, YEAR)))
+   d. CALCULATE(TOTALYTD(SUM('Sales'[SalesAmount]), SAMEPERIODLASTYEAR('Date'[Date])))

28. A data engineer is preparing data from a SQL database for a lakehouse in Microsoft Fabric. The source table contains a column named Price with some empty string values that need to be replaced with 0. The engineer wants a low-code solution that they can easily integrate into a data pipeline. What is the most appropriate action for the data engineer to take?

+   a. Write a Spark notebook with PySpark to read the data and replace the values.
+   b. Add a dataflow to the data pipeline and use its transformation capabilities to replace the empty strings.
+   c. Use a T-SQL query with an UPDATE statement to replace the empty strings.
+   d. Use a COPY INTO command to load the data and handle the transformation.

29. An analytics engineer is designing a new Power BI semantic model that needs to handle a very large dataset from a Fabric data warehouse. The report must provide fast interactive performance for

aggregations and queries. The data warehouse is scheduled to refresh nightly, so data freshness is not a major concern.

Which storage mode should the analytics engineer choose for the semantic model?

+   a. Import
+   b. DirectQuery
+   c. Direct Lake
+   d. Live Connect

30. A data team uses a single Microsoft Fabric workspace named Analytics for development and testing. The team needs to transition its work to a Production workspace, and also ensure that the business users can deploy their semantic models and reports from Analytics to Production in a controlled manner.

Which tool should the team use?

+   a. A data pipeline
+   b. Git integration
+   c. A deployment pipeline
+   d. A notebook

31. You’re creating a dimensional model for a new analytics solution. The model contains a Sales fact table and several dimension tables, including Supplier and Customer. To improve query performance and simplify the model for business users, you want to combine the Supplier and Customer dimension tables into a single table.

Which of the following is the most appropriate way to achieve this?

+   a. Denormalize the data by creating a single, wide table that combines data from all dimension tables into the Sales fact table.
+   b. Create a conformed dimension by combining the tables and ensuring all columns are in a single, well-defined table.
+   c. Create a role-playing dimension by aliasing one of the tables.
+   d. Create a new view in the data warehouse by joining the Supplier and Customer tables.

32. A team of data analysts is preparing to transform data from a source database. It needs to create a measure that calculates the number of days since the last order for each customer. The team wants a solution that allows for a step-by-step approach to data transformation and is easy to debug.

Which of the following is the best tool to use for this task?

+   a. A data pipeline with a lookup activity
+   b. A T-SQL script in the SQL query editor
+   c. A Dataflow Gen2 using Power Query
+   d. A Spark notebook with PySpark

33. A data engineer has a semantic model in Power BI with a Sales fact table that is joined to a Date dimension table. The engineer wants to ensure that a user who has access to only one specific product

category can only see the sales data for that category. The solution must use the least-privilege principle.

Which of the following is the most appropriate action to take?

+   a. Create a new semantic model for each product category with a filtered Sales table.
+   b. Implement object-level security (OLS) on the Sales fact table.
+   c. Implement row-level security (RLS) on the Sales fact table to filter the rows.
+   d. Implement column-level security (CLS) on the Sales fact table to hide the SalesAmount column.

34. A data engineer is working on a data warehouse in Microsoft Fabric. They need to ensure that the data loading process is as efficient as possible. To do this, they decide to use a MERGE statement. What’s the primary purpose of using a MERGE statement in this scenario?

+   a. To join two or more tables together to produce a single results set
+   b. To perform a bulk load of new data from a single source file
+   c. To combine INSERT, UPDATE, and DELETE operations into a single statement
+   d. To create a new table from the results of a query

35. You’re developing a new semantic model in Power BI, and you want to use a DAX measure to calculate the total sales for a specific product category. To make the DAX code more readable and easier to debug, you want to store the results of an intermediate calculation in a temporary variable. Which DAX keyword should you use for this purpose?

+   a. VAR
+   b. SET
+   c. DEFINE
+   d. LET

36. A data engineer is designing a dimensional model for a new analytics solution. The model contains a fact table with measures such as SalesAmount and a Product dimension table that contains attributes like ProductCategory. The engineer wants to make the model as simple and efficient as possible for a semantic model.

Which of the following best describes the relationship between the fact and dimension tables?

+   a. The fact table contains the measures, and the dimension table contains the primary keys.
+   b. Both the fact table and the dimension table contain measures and attributes.
+   c. The fact table contains the primary keys, and the dimension table contains the measures.
+   d. The fact table contains the foreign keys and measures, and the dimension table contains the attributes and a primary key.

37. A data analyst is working with a large sales dataset in a Fabric data warehouse.

The analyst needs to apply a series of simple transformations, such as changing a column’s data type, renaming columns, and removing duplicates.

They want to use a low-code approach that is easy to repeat.

What is the most appropriate tool to use?

+   a. A SQL notebook with Python code
+   b. A SQL query in the SQL query editor
+   c. The visual query editor in the Fabric data warehouse
+   d. A data pipeline with a copy activity

38. A data engineer is designing a dataflow to ingest and transform data. To ensure data quality and integrity, they want to enforce a rule that rejects any rows where the value in the ProductID column is NULL. Where can the data engineer enforce this data quality rule?

+   a. In the data source’s properties
+   b. In a SQL WHERE clause in the target table
+   c. In the data destination’s schema
+   d. In a data quality feature within Dataflow Gen2

39. An analytics engineer is designing an incremental refresh policy for a large semantic model. The model contains a Sales fact table that is partitioned by month. The team wants to keep the last three years of data and only refresh the current and previous month’s data. Which two parameters must the analytics engineer configure to achieve this?

+   a. DateFrom and DateTo
+   b. RangeStart and RangeEnd
+   c. DateTimeFrom and DateTimeTo
+   d. RefreshInterval and HistoricalPeriod

40. A data analyst needs to create a DAX measure that calculates the sales for the previous month. The measure should respect any filters on the Date table. Which of the following DAX functions should they use?

+   a. SAMEPERIODLASTYEAR
+   b. PREVIOUSMONTH
+   c. TOTALMTD
+   d. DATEADD

41. A data analyst is building a Power BI report using a large dataset that is split across a lakehouse and an external Azure SQL database. To provide a unified view for reporting, they need to create a semantic model that can query both sources directly without importing the data. Which feature of a semantic model should the analyst use?

+   a. A live connection
