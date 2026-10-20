
This is useful when we only want to work with complete data (for example, only orders that have already shipped). Figure 3-6 shows you visually what an INNER JOIN looks like.

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_125_0.png)

Figure 3-6. INNER JOIN of the order and shipment table, with only rows found in both tables kept

## LEFT OUTER JOIN

A LEFT OUTER JOIN keeps all the rows from the first (left) table, even if no match is found in the second (right) table. In our example, the following apply:

- All orders are included.
- If a shipment is available, the shipped date is shown.
- If not, the shipment fields will be empty (NULL).

This is useful when you want to see all orders, including the ones that haven’t shipped yet. Figure 3-7 shows you visually what a LEFT OUTER JOIN looks like.

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_126_0.png)

Figure 3-7. LEFT OUTER JOIN of the order and shipment table, with all rows from the left table and matching rows from the right table kept

## RIGHT OUTER JOIN

A RIGHT OUTER JOIN is the opposite of a LEFT OUTER JOIN. It keeps all rows from the second (right) table, even if there’s no match in the first (left) table. In our example, the following apply:

- All shipments are included.
- If there’s a matching order, its details are shown.
- If not, the order fields will be empty (NULL).

This is less common but is sometimes used when the second table is the main focus.

Figure 3-8 shows you visually what a RIGHT OUTER JOIN looks like.

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_127_0.png)

Figure 3-8. RIGHT OUTER JOIN of the order and shipment table, with all rows from the right table and matching rows from the left table kept

## FULL OUTER JOIN

A FULL OUTER JOIN keeps all rows from both tables. Where there’s a match, the data is combined, and if there’s no match, the missing values are shown as empty. In our example, you get every order and every shipment.

Orders without shipments and shipments without orders are both shown, with empty fields where data is missing. This is useful in audits and quality checks to identify mismatches. Figure 3-9 shows you visually what a FULL OUTER JOIN looks like.

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_128_0.png)

Figure 3-9. FULL OUTER JOIN of the order and shipment table, with all rows kept

## Joins in a Fabric Warehouse

In a Fabric warehouse, you work with structured tables using T-SQL. Let’s use our example again, in which we have an order table and a shipment table. Suppose you want to list all orders and show their shipment date if available. You’ll use a LEFT JOIN for this:

```
SELECT
    o.OrderID,
    o.OrderDate,
    o.InvoiceDate,
    s.ShippedDate
FROM
    orders o
LEFT JOIN
    shipments s
ON
    o.OrderID = s.OrderID;
```

If you’re new to SQL, the code might look a bit confusing at first, so let’s break it down:

```
FROM
    orders o
```

This means we’re starting with the orders table (the left table in Figure 3-9). The o is an alias that we assign to the table; it makes the rest of the query easier to read, especially if we have many columns or joins. Whenever we refer to the orders table, we can just write o, instead of the full name:

```
LEFT OUTER JOIN
    shipments s
```

This means we want to join the shipment table, with the alias s, and the join we want to perform is a LEFT JOIN:

```
ON
    o.OrderID = s.OrderID
```

This is the most important line in any join since it tells SQL how the two tables are connected. In this case, the OrderID column appears in both tables, and it represents the same thing: the ID of the order. This is called the join condition, and it must always be written after ON.

In our SQL query, all rows from the orders are included. If there’s a shipment with the same OrderID, the ShippedDate is shown; if not, the ShippedDate column will be empty (NULL). This is a very common join in business scenarios (for example, to show all orders and whether they’ve been shipped yet).

## Joins in a Fabric Lakehouse Using Notebooks

In the previous section, we used SQL in the Fabric warehouse to join two tables. But if we work with a lakehouse, we can use notebooks to perform the same operation. Let’s continue with our example of orders and shipments. We want to join them using a LEFT JOIN, so that we keep all orders, even if no shipment has been registered yet.

First, we define a temporary view using Spark SQL, and then, we use PySpark to query the result:

```
%%sql
CREATE OR REPLACE TEMP VIEW order_shipments AS
SELECT o.OrderID,
    o.OrderDate,
    o.TotalAmount,
    s.ShippedDate,
    s.Carrier
FROM orders o
LEFT JOIN shipments s
  ON o.OrderID = s.OrderID;
```

This query creates a temporary view that combines both tables. For each order, we want to add the shipment information; however, if there is no shipment yet, those fields will be NULL. After creating the view, we can query it by using PySpark:

```
df = spark.sql("SELECT * FROM order_shipments")
display(df)
```

The df = spark.sql("SELECT * FROM order_shipments") line runs a SQL query inside the Fabric notebook using the Spark engine. It selects all the data from the temporary order_shipments view, which was previously created. The result of this query is stored in a variable named df, which is a Spark DataFrame. Once the data is loaded into this DataFrame, the next line, display(df), shows the contents of that DataFrame directly in the notebook.

Alternatively, you can perform the join directly in PySpark without creating a SQL view:

```
df_orders = spark.table("orders")
df_shipments = spark.table("shipments")

df_joined = df_orders.join(df_shipments, df_orders.OrderID == df_shipments.OrderID, "left")
display(df_joined)
```

The df_joined = df_orders.join(...) line runs a join operation entirely in PySpark, returning a Spark DataFrame. This shows the same result as the SQL approach, but it provides more flexibility if you want to apply further transformations programmatically.

In a Fabric lakehouse, using notebooks is a powerful way to work with large datasets. You can join tables using Spark SQL or PySpark, making it easy to combine data, enrich it, and prepare it for analysis or further transformation.

## Joins in a Fabric Lakehouse Using Dataflow Gen2

A more visual and code-free way to join data is available through Dataflow Gen2. Within the lakehouse, we can

create a new Dataflow Gen2 by selecting one of our tables and then use the Merge queries option on the home tab (see Figure 3-10).

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_132_0.png)

Figure 3-10. Merging queries, either in the current table or as a new table

Once you click Merge queries, you’ll be asked to provide the second table (the right table) and then indicate the matching key by clicking on the columns, as shown in Figure 3-11. You can indicate multiple columns by holding down the Ctrl key on your keyboard while clicking on the columns.

## Merge

Select a table and matching columns to create a merged table.

orders

| OrderID | OrderDate | TotalAmount |
| --- | --- | --- |
| 1 | 5-1-2026 | 150 |
| 2 | 5-3-2026 | 200 |
| 3 | 5-4-2026 | 150 |

shipments

| ShipmentID | OrderID | ShippedDate | Carrier |
| --- | --- | --- | --- |
| S001 | 1 | 5-2-2026 | DHL |
| S002 | 2 | 5-5-2026 | FedEx |

Join Kind

Left Outer (all from first, matching from second)

Use fuzzy matching to perform the merge

Fuzzy matching options

The selection matches 2 of 3 rows from the first table.

Figure 3-11. Selecting the second table in a pop-up and indicating the column where the merge should happen

After you choose the tables to join and specify the type of join, you’ll see at the bottom of the pop-up window how many rows match between the tables. In our example, two out of three rows match. When you confirm the merge and expand the related columns, such as ShippedDate, the row without a match will show a NULL value for ShippedDate (see Figure 3-12).
