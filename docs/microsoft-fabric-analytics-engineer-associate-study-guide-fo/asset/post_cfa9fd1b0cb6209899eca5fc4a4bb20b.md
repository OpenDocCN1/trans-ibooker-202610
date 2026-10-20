
# NOTE

You may wonder what happens when the underlying data changes. Since a view is based on a live SQL query, it always reflects the latest state of the underlying tables. If the structure of your data changes—for instance, if you rename a column or add new logic—you update the view definition and all reports using that view will benefit from the updated logic instantly.

In Microsoft Fabric, you can create views directly within a lakehouse or warehouse by using the SQL analytics endpoint. Once you create views, you can query them interactively, use them in pipelines, and load them into Power BI by using either Import or Direct Lake mode, just like regular tables.

To illustrate how to create a view, we can access the SQL analytics endpoint of either our Fabric lakehouse or our Fabric warehouse. In this example, we use the sales sample data provided by Microsoft in a Fabric lakehouse. We want to create a view that combines the customer key and customer from the customer table with the profit and number of transactions from the sales table. We can achieve this by opening our Fabric lakehouse, clicking on New SQL Query, and writing the following code:

```
CREATE VIEW dim_customer AS
SELECT
    c.CustomerKey,
    c.Customer,
    SUM(fs.Profit) AS Profit,
    COUNT(fs.SaleKey) AS NumberOfTransactions
FROM fact_sale fs
JOIN dimension_customer c
ON fs.CustomerKey = c.CustomerKey
GROUP BY c.CustomerKey, c.Customer;
```

When we write this query and click on Run, we create a new view. We can find in the navigation pane on the left side of the lakehouse screen, which includes tables, views, and the query we used to create the views (see Figure 3-1).

Views are great when you want to simplify data access, abstract business logic, or control what information is shown to different users. However, views are limited to returning result sets; they don’t accept parameters or perform row-by-row operations. When your data transformation needs to be more advanced—for example, when calculations depend on input values or you need to use them in different queries—is when functions come in.

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_111_0.png)

Figure 3-1. The SQL analytics endpoint of the Fabric lakehouse

Let’s look at how functions work, what types exist, and how you can use them in a Fabric lakehouse or warehouse.

## Functions

A *function* is a small, reusable block of SQL code. It takes an input (which is called a *parameter*), performs an operation on it, and returns a result. Functions are useful because they help you avoid repeating the same logic over and over again. If a certain calculation or transformation comes up often, turning it into a function means you only need to write and update it once.

There are two main function types: *scalar functions*, which return a single value, like a discount or a formatted date; and *table-valued functions*, which return a full table that you can use again in queries. While traditional SQL allows both scalar functions and table-valued functions, Fabric currently supports only *inline table-valued functions*. These functions return a result set, typically based on the input values, and are written as a single RETURN SELECT statement. This means you cannot use BEGIN ... END blocks, declare variables, or traditionally use conditional logic. However, you can still express quite a bit of logic using a CASE statement or expressions within the SELECT.

### Scalar functions

Let’s look at an example in which we define a function that calculates a discount based on the input sales amount. If the amount exceeds $1,000, we return a 5% discount; otherwise, we return zero:

```
CREATE FUNCTION fn_CalculateDiscount (@SalesAmount DECIMAL(10,2))
RETURNS TABLE
AS
RETURN
    SELECT
        CASE
            WHEN @SalesAmount > 1000 THEN @SalesAmount * 0.05
            ELSE 0
        END AS Discount;
```

After we run the query, we can see a new function in the Functions folder of our warehouse (see Figure 3-2).

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_113_0.png)

Figure 3-2. Query to define a function to create a discount of 5% when the sales amount is higher than $1,000

Once you’ve created this function, you can use it in a SELECT statement by passing a value as the parameter:

```
SELECT Discount
FROM fn_CalculateDiscount(1500)
```

```
SELECT Discount
FROM fn_CalculateDiscount(999)
```

The result for the top statement is 75, which is 5% of 1,500, while the number returned for the second statement is 0. Since 999 is below the 1,000 threshold, no discount applies. You can then use the value returned by this function in other SQL queries or views, for example, to calculate the total discount per customer or add discount logic to a sales report.

### Table-valued functions

Now, let’s now look at an example of a table-valued function, which returns multiple rows from a table based on input parameters. Table-valued functions are especially useful when you want to return a filtered or calculated subset of data that’s based on one or more values you provide. Imagine you often need to retrieve high-value sales, meaning those where the sales amount exceeds a certain threshold. Rather than repeating the same filtering logic in every query, you can create a reusable function for that logic.

Here’s an example of a table-valued function that accepts a minimum sales amount as input and returns all rows from the fact_sale table in which the total meets or exceeds the minimum amount. This example is based on the sales demo data provided in the Microsoft Fabric Lakehouse:

```
CREATE FUNCTION fn_GetHighValueSales (@MinAmount DECIMAL(10,2))
RETURNS TABLE
AS
RETURN
    SELECT
        SaleKey,
        CustomerKey,
        Profit,
        TotalExcludingTax,
        InvoiceDateKey
    FROM fact_sale
    WHERE TotalExcludingTax >= @MinAmount
```

This function accepts a single parameter: @MinAmount. It returns a table that contains all sales records in which TotalExcludingTax is greater than or equal to the amount passed in. The returned table includes selected columns such as the sale key, customer key, profit, and invoice date that may be relevant when you’re analyzing high-value transactions.

After creating the function, you can use it in a query like you would any other table or view:

```
SELECT *
FROM fn_GetHighValueSales(500)
```

This query will return all sales with a total of $500 or more. The primary benefit of this is that the filtering logic is stored in one place (within the function), which makes it easy to reuse and maintain. If you ever want to change the threshold logic or add more filters, you only need to update the function once, instead of editing every individual query where this logic appears.

## Stored Procedures

While views and functions are great for encapsulating reusable logic, *stored procedures* are ideal when you need to perform a series of steps or actions in a defined order. A stored procedure is a saved block of SQL statements that you can execute as a single unit. Unlike views and functions, which are primarily used to return data, stored procedures are typically used to perform actions, such as inserting data, updating tables, cleaning up staging data, and calling other procedures or functions.

Stored procedures are especially useful when you’re automating a repetitive data preparation process or when you want to package logic that’s too complex for a single SELECT statement.

Let’s say you have a table with new sales records that you need to process and insert into your main fact_sale table. In traditional relational databases, this type of logic, which involves inserting data and optionally logging or validating it, is stored in a stored procedure. Here’s an example of what a stored procedure might look like in a Fabric warehouse:

```
CREATE PROCEDURE sp_LoadSalesData
AS
BEGIN
    INSERT INTO fact_sale (SaleKey, CustomerKey, Profit,
TotalExcludingTax)
    SELECT SaleKey, CustomerKey, Profit, TotalExcludingTax
    FROM stg_sale;
END
```

It’s worth noting that Fabric lakehouses do support stored procedures, but with limited functionality. You can use them to run a fixed set of SQL commands, such as inserting data from one table into another. However, they don’t support control-flow logic, such as IF statements, WHILE loops, and variable usage. Additionally, you can’t call stored procedures in a lakehouse from notebooks or pipelines at this time. In practice, this means a stored procedure in a lakehouse acts more like a simple wrapper around a static SQL script. While this can be useful for organizing basic operations, it’s not as flexible or dynamic as a stored procedure in a Fabric warehouse.

In Microsoft Fabric, views, functions, and stored procedures help you structure and reuse logic efficiently. Views are virtual tables based on SQL queries. They don’t store data, but they simplify complex queries and control data access. Views are great when you want to simplify data access, abstract business logic, or control what information is shown to different users. Functions allow you to define reusable logic. Scalar functions return a single value, while table-valued functions return a full table. Functions are useful when you need consistent calculations or filtering logic across different queries or reports. Stored procedures group multiple SQL steps into a single unit, and they’re ideal for tasks like inserting data, cleaning up staging tables, and running data workflows. In warehouses, stored procedures are flexible and support T-SQL features. In lakehouses, stored procedures are supported but limited, and you can only run simple SQL scripts without logic, variables, or parameters. You also can’t call lakehouse-
