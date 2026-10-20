64. Correct answer: C
You should use a Dataflow Gen2 because it can copy data from sources like CSV files into a lakehouse and also apply Power Query M transformations during the process. This makes it suitable for both ingestion and data cleaning in a single, low-code activity.

65. Correct answer: A
We use calculation groups to simplify time intelligence calculations in Power BI. They allow us to define reusable logic for operations like year-to-date, previous month, and quarter comparisons and apply it across multiple measures. This reduces repetition and improves model maintainability.

66. Correct answer: B
In a dimensional model, the fact table stores numeric measures (like SalesAmount) and foreign keys that link to dimension tables. The dimension table stores descriptive attributes (like ProductCategory) and a primary key that matches the foreign keys in the fact table.

67. Correct answer: B
You follow best practices and modular design to make the solution performant, maintainable, and reusable. Modular design breaks complex transformations into smaller, reusable components, making it easier to manage and optimize.

68. Correct answer: B
You should use a star schema because it organizes the model with a central fact table (Sales) that’s