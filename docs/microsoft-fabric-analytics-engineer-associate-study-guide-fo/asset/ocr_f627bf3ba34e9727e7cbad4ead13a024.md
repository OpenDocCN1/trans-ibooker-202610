53. You’re designing a Dataflow Gen2 that ingests data from a CSV file and transforms it into a dimensional model. During transformation, you need to remove duplicate rows and ensure column types are consistent.

Which two transformation steps should you apply?

+   a. Remove duplicates from the source table.
+   b. Change column data types to match the dimensional model.
+   c. Merge queries by using fuzzy matching to enrich product names.
+   d. Fill missing values in numeric columns with zeros.
+   e. Trim leading and trailing spaces from text columns.

54. You’re building a pipeline that ingests data from an external SQL database into a lakehouse. The data includes a Price column that contains empty strings and NULL values. You want to replace the empty strings with 0 using a low-code solution, without affecting the NULL values.

What should you use?

+   a. A Spark notebook with PySpark
+   b. A Dataflow Gen2 transformation
+   c. A stored procedure in the warehouse
+   d. A T-SQL UPDATE statement in the pipeline