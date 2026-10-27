# Working in the Service

Some general tips for setting up a Q&A include the following:

+   - Revise any columns with incorrect data types, such as dates or numbers that are stored as text data types.
+   - Set the default summarization for dimension columns to **Do not summarize**.
+   - Associate geographical columns, such as states and zip codes, with a related data category.
+   - Split columns containing multiple values into distinct columns.
+   - Normalize tables such that entities within the tables are moved to their own distinct table; for example, columns of a vendor in a products table can be moved to a vendor table.
+   - Q&A queries only work with the active relationships of a model. Consider dedicated role-playing dimensions with active relationships. Alternatively, consider de-normalizing an inactive relationship dimension into a fact table.
+   - Define the top or most common natural language questions and test for accuracy in Power BI.
+   - Identify the gaps between the names of data model entities and the names used in natural language queries.
+   - Focus on entities with longer and less intuitive names that are not used casually by the users.
+   - Avoid reusing the same synonym across multiple entities, as this can lead to incorrect query results.
+   - Ensure that the primary synonym for each entity of the model is unique.

# There's more

Q&A may not be appropriate for certain dashboards and datasets. For example, the Q&A search bar **Ask a question about your data** may be a distraction to users who only want to view the KPIs and other information as presented. Additionally, if the dataset requires a gateway, such as an on-premises SSAS server or a DirectQuery Power BI Desktop model to an on-premises source, Q&A may be avoided, given the additional (and potentially inefficient) queries and performance considerations.

To enable or disable Q&A, follow these steps:

+   1. In the Power BI service, access the app workspace containing the dashboard.
+   2. Use the three vertical dots and click **Settings**.