taxi companies based on total miles traveled. If there’s a tie for total miles, you should include both or all tied companies.

Which T-SQL statements do you need to use to calculate the total miles per company before limiting the results?

+   a. TOP (3)
+   b. FETCH NEXT 3 ROWS ONLY
+   c. ORDER BY SUM(tripDistance) DESC
+   d. GROUP BY TaxiCompany

58. You’re working with a lakehouse table named ProductSales that includes the columns ProductID, SaleDate, and Revenue. You need to create a view that returns the total revenue per product for the current year only.

Which T-SQL clause should you use to filter the data?

+   a. HAVING YEAR(SaleDate) = YEAR(GETDATE())
+   b. WHERE YEAR(SaleDate) = YEAR(GETDATE())
+   c. GROUP BY YEAR(SaleDate)
+   d. FILTER BY CURRENT_YEAR

59. As a data engineer, you’re working with a Fabric warehouse that contains a table named CustomerTransactions. You need to create a T-SQL statement that returns the total transaction amount per customer for the current year.

Which combination of clauses should you use?