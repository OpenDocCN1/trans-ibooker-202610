# Connect to new dataset

+   - File upload
+   - Data lake/data store
+     - Amazon S3
+   - Database connections
+     - Amazon Redshift
+     - JDBC
+   - AWS Glue Data Catalog
+     - Data Catalog S3 tables
+     - Data Catalog Redshift tables
+     - Data Catalog RDS tables
+     - All AWS Glue tables
+   - Others
+     - Amazon AppFlow
+     - AWS Data Exchange
+   - External data connections
+     - Snowflake

![](bbox=[0.33, 0.087, 0.67, 0.615])

Figure 2.4: Data source list in the AWS Glue DataBrew console

**Recipe**: In AWS Glue DataBrew, a recipe is a set of transformation steps that you create or modify while working on a project. You can publish a recipe as a stand-alone entity. A published recipe consists only of the transformation steps, without any reference to the original data. You can share the published recipe by downloading it in YAML or JSON format. Recipes can have multiple published versions, which you publish from a project. You can also have multiple working copies of the same recipe, as you progress through multiple drafts.

![](bbox=[0.12, 0.75, 0.875, 0.895])