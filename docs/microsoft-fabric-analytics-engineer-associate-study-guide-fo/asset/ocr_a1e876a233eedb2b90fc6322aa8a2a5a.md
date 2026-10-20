# Aggregations to Summarize Data

*Aggregations* help you group your data and calculate meaningful totals, averages, and counts, and they can help you find minimum and maximum values. Common aggregation functions are as follows:

SUM

This adds up all values in a group.

COUNT

This counts the number of rows or non-NULL values.

AVG

This calculates the average value.

MIN

This finds the smallest value.

MAX

This finds the largest value.

Let’s look at two practical examples, total sales by product and number of customers by region, and how we can aggregate them in a Fabric warehouse by using SQL and a Fabric lakehouse with notebooks and Dataflow Gen2.

# Aggregation in a Fabric Warehouse with SQL

Using SQL, we group and aggregate data with the GROUP BY clause. That clause tells SQL which column(s) to group the rows by, while the aggregation functions like SUM and COUNT operate on the grouped rows to calculate the summary value. For example, if we want to see total sales