changes in the source and preventing downstream application failures due to such changes. Check out this blog post, which explains how to detect any changes in [the structures of your data source using AWS Glue](https://aws.amazon.com/blogs/big-data/identify-source-schema-changes-using-aws-glue/):

![](bbox=[0.12, 0.17, 0.88, 0.46])

Figure 6.7 – Table property in AWS Glue Data Catalog

![](bbox=[0.12, 0.5, 0.88, 0.75])

Figure 6.8 – Table property in AWS Glue Data Catalog

# Partitions

Partitioning is a strategy that’s used to optimize query performance by dividing a table based on the values of specific column(s). A table can have multiple partition keys that define a specific partition (also known as a partition specification). This technique improves query performance by reducing the amount of data that needs to be processed to answer a specific query: