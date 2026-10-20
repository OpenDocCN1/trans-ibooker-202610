
42. A data engineer has a warehouse with a table that contains sensitive financial data, including a Salary column. The engineer wants to ensure that a specific group of users can see the table but can’t see the Salary column. Which security measure should the data engineer implement?
- a. Dynamic row-level security
- b. Column-level security (CLS)
- c. Object-level security (OLS)
- d. Row-level security (RLS)

43. A data team needs to analyze streaming log data from an application. The logs are high volume and semi-structured, and they require quick, ad hoc queries. Which of the following is the most suitable approach to storing and querying this data in Microsoft Fabric?
- a. Ingesting the data into a data warehouse and querying it with T-SQL
- b. Ingesting the data into a lakehouse and querying it with T-SQL
- c. Ingesting the data into a KQL database and querying it with KQL
- d. Storing the data in a Data Lake and querying it with a Spark notebook

44. An analytics engineer is designing a new data solution in Microsoft Fabric. The solution will involve a central data store that is frequently accessed for T-SQL and Spark queries.

Data engineers need to be able to access semi-structured and unstructured data, and the data store must also support row-level security for users who are executing T-SQL queries.

Which type of data store is the most appropriate choice for this solution?
- a. A data lake
- b. A lakehouse
- c. A warehouse
- d. A KQL database

45. You have a Fabric workspace named Workspace1 that contains a lakehouse named Lakehouse1. You need to ensure that only members of GroupA can access sensitive columns in the lakehouse. The solution must use built-in Fabric governance features and follow least-privilege principles.

Which two actions should you perform?
- a. Apply a sensitivity label to the sensitive columns in Lakehouse1.
- b. Configure row-level security in Lakehouse1.
- c. Configure column-level security in Lakehouse1.
- d. Endorse Lakehouse1 as trusted content.
- e. Use workspace-level access controls to restrict GroupA.

46. You have a Fabric workspace named Workspace1 with a SQL warehouse named Warehouse1 and a semantic model named Model1. You need to deploy updated semantic model changes to production using code-based CI/CD from a third-party tool. What should you do to deploy the changes?
- a. Configure version control for Workspace1.
- b. Deploy the semantic model using the XMLA endpoint.
- c. Create a deployment pipeline between development and production.
- d. Publish Model1 directly from Power BI Service.
- e. Use .pbids files in the workspace.

47. You have data coming from multiple sources, and you need to select the appropriate Fabric storage method that supports both querying and file-based transformations. Which storage option should you choose?
- a. A lakehouse
- b. A warehouse
- c. An eventhouse
- d. An SQL database

48. You want to analyze and query data using Fabric KQL and DAX. Which two capabilities should you ensure are supported?
- a. Semantic models supporting DAX
- b. Lakehouse queries using SQL
- c. Eventhouse queries using KQL
- d. Dataflows supporting DAX
- e. Warehouses supporting KQL

49. You have a Fabric workspace, and you need to ingest real-time streaming data into your analytics solution while integrating with OneLake. Which component should you use?
- a. A lakehouse
- b. A warehouse
- c. An eventhouse
- d. A dataflow
- e. A semantic model

50. You’re building a semantic model, and you want to support dynamic formatting and calculation flexibility. Which two features should you implement?
- a. Calculation groups
- b. Dynamic format strings
- c. Field parameters
- d. Star schema relationships
- e. Incremental refreshes

51. You’re working with a lakehouse table named SalesTransactions that contains millions of rows. The table includes rows called TransactionDate, CustomerID, and Amount.

You need to create a view that filters out transactions with NULL amount values and calculates the total amount per customer per month.

How should you complete the T-SQL statement?
- a. Use SELECT INTO with a HAVING statement to create the summary table.
- b. Create a VIEW that uses a WHERE statement and GROUP BY.
- c. Create a PROCEDURE that uses GROUP BY to return the results set.
- d. Use MERGE with PARTITION BY to combine rows into a single dataset.

52. A data engineer needs to join two tables in a lakehouse: Orders and Products. The Orders table contains a ProductID column, and the Products table contains ProductID and Category columns.

What type of join should the data engineer use to include all orders, even if the product category is missing?
- a. An INNER JOIN
- b. A LEFT OUTER JOIN
- c. A RIGHT JOIN
- d. A FULL OUTER JOIN

53. You’re designing a Dataflow Gen2 that ingests data from a CSV file and transforms it into a dimensional model. During transformation, you need to remove duplicate rows and ensure column types are consistent.

Which two transformation steps should you apply?
- a. Remove duplicates from the source table.
- b. Change column data types to match the dimensional model.
- c. Merge queries by using fuzzy matching to enrich product names.
- d. Fill missing values in numeric columns with zeros.
- e. Trim leading and trailing spaces from text columns.

54. You’re building a pipeline that ingests data from an external SQL database into a lakehouse. The data includes a Price column that contains empty strings and NULL values. You want to replace the empty strings with 0 using a low-code solution, without affecting the NULL values.

What should you use?
- a. A Spark notebook with PySpark
- b. A Dataflow Gen2 transformation
- c. A stored procedure in the warehouse
- d. A T-SQL UPDATE statement in the pipeline

55. You’re working in a Dataflow Gen2 that ingests product data from a CSV file. The file contains inconsistent formatting, including leading and trailing spaces in product names and numeric columns imported as text. You need to clean the data before loading it into a lakehouse.

What two things should you do?
- a. Remove duplicates from the source table.
- b. Trim leading and trailing spaces from text columns.
- c. Fill missing values in numeric columns with zeros.
- d. Merge queries using fuzzy matching.
- e. Change column data types to match the dimensional model.

56. You’re working with a KQL database that contains a table named MeterReadings. You need to calculate the difference in reading values between each row and its previous row, grouped by area. Which KQL function should you use?
- a. prev()
- b. row_rank_min()
- c. summarize()
- d. window_session()

57. The TaxiTrips lakehouse table contains millions of rows, including columns named TaxiCompany and tripDistance. Your goal is to identify the top three taxi companies based on total miles traveled. If there’s a tie for total miles, you should include both or all tied companies.

Which T-SQL statements do you need to use to calculate the total miles per company before limiting the results?
- a. TOP (3)
- b. FETCH NEXT 3 ROWS ONLY
- c. ORDER BY SUM(tripDistance) DESC
- d. GROUP BY TaxiCompany

58. You’re working with a lakehouse table named ProductSales that includes the columns ProductID, SaleDate, and Revenue. You need to create a view that returns the total revenue per product for the current year only.

Which T-SQL clause should you use to filter the data?
- a. HAVING YEAR(SaleDate) = YEAR(GETDATE())
- b. WHERE YEAR(SaleDate) = YEAR(GETDATE())
- c. GROUP BY YEAR(SaleDate)
- d. FILTER BY CURRENT_YEAR

59. As a data engineer, you’re working with a Fabric warehouse that contains a table named CustomerTransactions. You need to create a T-SQL statement that returns the total transaction amount per customer for the current year.

Which combination of clauses should you use?
