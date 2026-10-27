+   7. Move the **Reseller Sales** query to a **Facts** query group and the **Resellers** and **Dates** queries to a **Dimensions** query group
+   8. In the **Other Queries** query group, create a blank query called **Calculations**
+   9. Choose **Close & Apply** from the ribbon of the **Home** tab
+   10. Save your work

# How to do it...

To implement this recipe, use the following steps:

+   1. Create the following base measures in the **Calculations** table:
+      `Reseller Gross Sales = SUM('Reseller Sales'[SalesAmount])`
+      `Reseller Discount Amount = SUM('Reseller Sales'[DiscountAmount])`

+   2. Create a **Reseller Margin %** measure in the **Calculations** table:
+      `Reseller Margin % =`
+      `/*`
+      `Net Sales = Gross sales net of discounts that have shipped`
+      `Product Cost = Product standard cost of all ordered products (including not shipped). A Date of 12/31/2099 used for unshipped sales order lines since 1/1/2015`
+      `*/`
+      `VAR ShippedSales = CALCULATETABLE('Reseller Sales', 'Reseller Sales'[ShipDate] <> DATEVALUE("12/31/2099"))`
+      `VAR NetSalesShipped = CALCULATE([Reseller Gross Sales] - [Reseller Discount Amount],ShippedSales)`
+      `VAR ProductCost = SUMX('Reseller Sales', 'Reseller Sales'[OrderQuantity] * 'Reseller Sales'[ProductStandardCost])`
+      `RETURN`
+      `DIVIDE(NetSalesShipped - ProductCost,NetSalesShipped)`

+   3. Create a **Reseller Gross Sales (Custom)** measure in the **Calculations** table:
+      `Reseller Gross Sales (Custom) =`
+      `VAR CurrentDate = MAX('Dates'[FullDateAlternateKey])`
+      `VAR ResellerTypes = CALCULATETABLE('Resellers', Resellers[BusinessType] = "Warehouse")`
+      `VAR DateHistory = --Trailing 10 Days`
+      `FILTER(ALL('Dates'),'Dates'[FullDateAlternateKey] <= CurrentDate && 'Dates'[FullDateAlternateKey] >= CurrentDate - 10)`
+      `RETURN`
+      `CALCULATE([Reseller Gross Sales],ResellerTypes,DateHistory)`