
![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_134_0.png)

*Figure 3-12. Results of a LEFT OUTER JOIN with one value not available in the second table*

The result is a combined table that is ready for reporting or further transformation. Thanks to the intuitive visual interface, you don’t need to write any code to perform these joins. This makes working with related data accessible for beginners and those who are less familiar with SQL or Spark. You may also notice join types called LEFT ANTI and RIGHT ANTI in the merge options (refer back to Figure 3-11). These join types are useful when you want to find rows in one table that do not have a corresponding match in the other table; this helps you quickly identify unmatched or missing data.

Whether you use SQL in the Fabric warehouse or notebooks and Dataflow Gen2 in the Fabric Lakehouse, you need to understand joins and merges. It helps you to bring together related data from different sources and create tables that support clear and insightful reporting and analysis.

## Summarizing Data

After you combine your data by using joins, the next important step is to summarize it.

## Aggregations to Summarize Data

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

## Aggregation in a Fabric Warehouse with SQL

Using SQL, we group and aggregate data with the GROUP BY clause. That clause tells SQL which column(s) to group the rows by, while the aggregation functions like SUM and COUNT operate on the grouped rows to calculate the summary value. For example, if we want to see total sales

per product, we group the data by the ProductName column. The SUM(SalesAmount) part calculates the total sales within each group:

```
SELECT
    ProductName,
    SUM(SalesAmount) AS TotalSales
FROM
    sales
GROUP BY
    ProductName;
```

This query groups the sales data by product (ProductName) and calculates the total sales amount for each product using SUM. The result is a table that shows each product with its total sales.

Similarly, to count customers by region, we group by the Region column and use the COUNT function in the CustomerID column to find the number of customers in each region:

```
SELECT
    Region,
    COUNT(CustomerID) AS NumberOfCustomers
FROM
    customers
GROUP BY
    Region;
```

Here, the query groups rows by Region and counts the number of customer IDs in each group.

> **NOTE**
> These SQL aggregation queries also work in a Fabric lakehouse using Spark SQL. In this chapter, we focus on the warehouse examples for clarity, but the same operations are possible in both environments.

## Aggregation in a Fabric Lakehouse with Notebooks

In a notebook in a Fabric lakehouse, we use PySpark and work with DataFrames. To perform aggregations, we use the .groupBy() method to define the column(s) to group by, followed by an aggregation function like .sum(), .count(), or .agg(). Let’s say we want to calculate total sales per product. Here’s how we do it in PySpark:

```
sales_df.groupBy("ProductName").sum("SalesAmount").show()
```

The .groupBy("ProductName") method groups the data by the product name, .sum("SalesAmount") calculates the total sales per group, and .show() displays the result in the notebook. The results show each product with the corresponding sum of sales (see Figure 3-13).

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_137_0.png)

Figure 3-13. Grouping sales by product in a notebook

To count the number of customers by region, we can do something similar:

```
customers_df.groupBy("Region").count().show()
```

In this example, .count() counts how many rows (customers) are in each region. Since we’re using .count() directly after .groupBy(), it returns the total number of rows in each group—which in this case is each region.

## Aggregation in a Fabric Lakehouse with Dataflow Gen2

In Dataflow Gen2, aggregations are done visually through transformations in the Power Query editor. In our lakehouse, we can create a new Dataflow Gen2 and load our tables, and once we have the tables, we can click on Transform to reveal the Group by button (see Figure 3-14).

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_138_0.png)

Figure 3-14. Clicking on Transform reveals the Group by button

Then, we can click on “Group by” to define how we want to group. For example, we can group by ProductName and then say what should happen, like summarizing the SalesAmount (see Figure 3-15).

# Group by

Specify the column to group by and the desired output.

Basic Advanced

Group by *
ProductName

New column name *
Sales
Operation *
Sum
Column *
SalesAmount

Use fuzzy grouping

Fuzzy group options

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_139_0.png)

Figure 3-15. Summarizing the sales amount by product name

The result will be the Product in one column and the SalesAmount in the other.

After combining data using joins, aggregation is the next crucial step in transforming detailed data into meaningful summaries. Aggregations allow you to group rows by one or more columns and apply functions like SUM, COUNT, AVG, MIN, and MAX to calculate totals, averages, counts, or extremes within each group. In the Fabric warehouse, SQL’s GROUP BY clause groups data while aggregation functions calculate summary values per group. In the Fabric lakehouse, notebooks utilize the .groupBy() method with aggregation functions such as .sum() and .count() to achieve the same result. Dataflow Gen2 enables visual grouping and aggregation via the Power Query “Group by” transformation, making summarizing accessible without code.

Whether you work in the Fabric warehouse or the lakehouse, adding new columns or tables to your data helps you keep pace with changing business needs.

## Filters and Focusing on Relevant Data

Another important step when working with data is filtering it to make it as narrow and relevant as possible. We filter our tables to include only rows that meet certain criteria, because it enhances performance and makes analysis more straightforward. Depending on where we handle our data, we can use different filtering approaches:

- The WHERE clause in SQL, which specifies conditions rows must meet to be included
- Filtering rows in notebooks, using Spark/PySpark DataFrames with conditional expressions
- Visual filters in Dataflow Gen2, using Power Query transformations

## Filtering Data

Let’s have a look at how to apply filters in a Fabric warehouse using SQL, in a Fabric lakehouse using notebooks, and visually in Dataflow Gen2.

## Filtering in a Fabric Warehouse with SQL

In SQL, you perform filtering with the WHERE clause, which allows you to specify conditions for rows to be selected. For example, if you only want to look at sales orders from the last 30 days, you can filter based on the order date:

```
SELECT *
FROM sales
WHERE
OrderDate >= DATEADD(day, -30, CURRENT_DATE);
```

This query returns all rows where the OrderDate is within the 30 days prior to today. To filter sales where the amount is greater than $100, the query would be as follows:

```
SELECT  *
FROM sales
WHERE SalesAmount > 100;
```

The WHERE clause tells SQL to include only rows where the condition is true.

## Filtering in a Fabric Lakehouse with Notebooks

In PySpark notebooks, you perform filtering by applying conditional expressions on DataFrames using the .filter() or .where() method. Both .filter() and .where() work the same way, and you can chain them with other transformations. For example, to filter orders from the last 30 days, do the following:

```
from pyspark.sql.functions import current_date, date_sub

recent_sales_df = sales_df.where(sales_df.OrderDate >= date_sub(current_date(), 30))
recent_sales_df.show()
```

Here, .filter() keeps only rows where the OrderDate is within the last 30 days.

> **NOTE**
> 
> In Microsoft Fabric notebooks, you can use both SparkSQL and PySpark to filter and transform data. While SparkSQL uses SQL-like syntax and PySpark uses Python-based DataFrame operations, the two are interchangeable for most tasks. Throughout this chapter, we’ll show examples using PySpark.
