+   17. Click the **Save** button.
+   18. Click the **Manage terms** tab and notice that **wholesaler** and **wholesaler net sale** are now defined as terms with the correct definitions.
+   19. Click on the **Field synonyms** tab, expand the **Resellers** table and note that the **wholesaler** synonym has been added for the **Resellers** table.

![](bbox=[0.2, 0.19, 0.87, 0.36])

Figure 5.18: Table synonym

20. Expand the **Reseller Sales** table, scroll down, and see that the **wholesaler net sale** synonym has been added to the **Reseller Net Sales** measure.

![](bbox=[0.2, 0.43, 0.87, 0.54])

Figure 5.19: Measure synonym

21. Close the **Q&A setup** dialog using the close button in the upper-right corner.

# How it works

Power BI Q&A supports Import mode and live connections to Azure Analysis Services, SQL Server Analysis Services (with a gateway), and Power BI datasets. RLS is supported for those data sources. Additionally, Q&A supports DirectQuery to SQL DirectQuery sources, such as Azure SQL Database, Azure Synapse Analytics, and SQL Server 2019. Q&A is not supported for composite data models or Reporting Services.

Synonyms are a crucial part of making sure that business users find the Q&A experience easy and intuitive—as well as getting the right answers to their questions. Synonyms allow a semantic layer to be added to the data model that uses business terminology instead of purely just the names of tables, columns, and measures within the model. Tables, columns, and measures can be included or excluded from Q&A and can have synonyms defined.