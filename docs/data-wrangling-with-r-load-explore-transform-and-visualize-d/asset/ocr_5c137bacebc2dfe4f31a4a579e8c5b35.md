values, and the average length of the values. This information is important for query optimization and performance tuning:

![](bbox=[0.12, 0.15, 0.88, 0.6])

Figure 6.18 – AWS Glue Catalog – Advanced properties

Table stats can be viewed and updated through the AWS Glue console or the Glue API. They can also be automatically gathered by AWS Glue Crawler during the data discovery process.

Table stats in Glue Data Catalog provide important metadata about the tables stored in your data catalog. These stats include information such as the number of rows in a table, the size of the table, and the column statistics, such as minimum and maximum values, data distribution, and data type. These statistics play a crucial role in query optimization and performance tuning within AWS Glue.

These table statistics are utilized by AWS Glue and query engines such as Amazon Athena or Amazon Redshift Spectrum to make informed decisions about query execution plans. Here’s how table stats are important and can be leveraged for query optimization and performance tuning:

+   - **Query planning:** The query optimizer uses table statistics to estimate the cardinality (number of rows) and data distribution of tables involved in a query. This estimation helps the optimizer choose the most efficient query execution plan. Accurate statistics enable the optimizer to make better decisions, resulting in faster and more efficient query processing.