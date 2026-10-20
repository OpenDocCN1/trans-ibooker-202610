+   a. CALCULATE(SUM(Sales[Amount]), PREVIOUSYEAR(Date[Date]))
+   b. SAMEPERIODLASTYEAR(SUM(Sales[Amount]))
+   c. TOTALYTD(Sales[Amount], Date[Date])
+   d. CALCULATE(SUM(Sales[Amount]), SAMEPERIODLASTYEAR(Date[Date]))
+   e. DATEADD(Date[Date], -1, YEAR)

74. As a data engineer, you’re designing a semantic model that includes a Customer dimension table and a Sales fact table. You want to ensure that users with access to only one customer can only see the sales data for that customer.

Which kind of security should you implement?

+   a. Object-level security (OLS)
+   b. Row-level security (RLS)
+   c. Column-level security (CLS)
+   d. Calculation groups
+   e. Composite models

75. You’re designing a semantic model that includes both Import and DirectQuery tables. You want to allow users to query both sources in the same report while maintaining performance. What should you use?

+   a. A composite model and dual storage mode
+   b. Calculation groups and Import mode
+   c. Field parameters and Direct Lake