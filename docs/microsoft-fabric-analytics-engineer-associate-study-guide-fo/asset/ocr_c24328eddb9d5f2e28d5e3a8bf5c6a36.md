This is especially useful in financial reporting, where measures may have different formats, such as currency and percentages.

73. Correct answer: D

This DAX measure calculates total sales for the same period last year and keeps any filters applied in the report, like specific months or products. CALCULATE applies the filter, and SAMEPERIODLASTYEAR moves the dates to the previous year.

74. Correct answer: B

RLS is used to restrict access to specific rows in a table based on user permissions. In this scenario, it ensures that a user who’s assigned to a particular customer can only see sales data for that customer. Using RLS is the correct approach in Power BI because it provides row-by-row access control, which ensures that sensitive customer data is protected while allowing users to interact with the model normally for the data they are allowed to see.

75. Correct answer: A

A composite model allows you to combine Import and DirectQuery tables in the same Power BI model. Using dual storage mode for tables lets some tables use Import for fast in-memory queries while others use DirectQuery to access live data. This approach provides both flexibility and performance in the same report.

76. Correct answer: B

A custom fiscal calendar table allows you to define a fiscal year, quarter, and month according to the