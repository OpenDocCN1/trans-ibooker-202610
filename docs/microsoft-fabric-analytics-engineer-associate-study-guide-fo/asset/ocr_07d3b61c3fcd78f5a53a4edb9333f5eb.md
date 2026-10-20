# DAX INFO.VIEW Functions

When working with Power BI, understanding your data is important for visualization. However, grasping the semantic model, including its structure and performance, is equally important. Gaining these insights will help you produce better documentation and achieve simpler maintenance and smarter performance optimization.

Here are a few questions that may come to mind when working with a Power BI report:

+   - What DAX measures exist in the model?
+   - What are the DAX expressions in the measures?
+   - Are there hidden tables or columns that need review?
+   - What are the relationships among the different tables?

You could answer the first question (What DAX measures exist in the model?) by manually clicking through each DAX measure and looking at the syntax. But that quickly becomes tedious, especially in larger models. A much faster and more scalable option is to use the INFO.VIEW functions in Power BI, which return the semantic model’s metadata in a tabular format. This approach is useful for documentation and governance, but it’s also great for spotting optimization opportunities, such as unused columns and overly complex measures.

Let’s take a look at some examples of how to retrieve information about the measures, columns, tables, and relationships we use. First, we’ll look into retrieving this data using the DAX query view, and after that, we’ll look