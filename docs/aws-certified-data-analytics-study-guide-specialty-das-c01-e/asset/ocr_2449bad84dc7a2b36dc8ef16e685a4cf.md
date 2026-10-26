and project, filter, join, and aggregate based on the need and then send the results back to Amazon Redshift, where final aggregation takes place and a join with a Redshift table is done in the Redshift cluster. Results of the query are then sent back to the client, which can be a BI tool or a query editor.

Figure 4.20 depicts the life of a query when querying Amazon Redshift and Amazon Redshift Lakehouse and hopefully makes the concept a bit clearer.

# FIGURE 4.20 Life of a query

![](bbox=[0.22, 0.27, 0.85, 0.57])

Amazon Redshift Lakehouse seamlessly integrates with your existing SQL and BI applications and supports the following key capabilities, which make it a very useful tool in your analytics arsenal:

+   - Support for American National Standards Institute (ANSI) SQL
+   - Support for a variety of file formats on your data lake, including Parquet, ORC, JSON
+   - Ability to read compressed and encrypted files
+   - Ability to push down optimization to your data lake without loading any data to Redshift
+   - Support for complex joins, nested queries, and window functions often used in analytical queries
+   - Support for partitioned data on S3, including dynamic partition pruning
+   - Ability to leverage the Glue Data Catalog and Amazon EMR Hive Metastore
+   - Ability to use Redshift’s cost-based optimizer