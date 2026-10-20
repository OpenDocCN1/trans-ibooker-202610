provide. Imagine you often need to retrieve high-value sales, meaning those where the sales amount exceeds a certain threshold. Rather than repeating the same filtering logic in every query, you can create a reusable function for that logic.

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