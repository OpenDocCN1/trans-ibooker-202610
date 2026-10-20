![](bbox=[0.14, 0.08, 0.85, 0.33])

*Figure 3-48. Expanding the relevant columns from the merged table*

Before you load the tables, you want to ensure that the Product detail table is no longer visible in your Power BI data model. It’s also important to note that you can’t simply remove the table, as your merge would then fail. What you need to do is go to the Product detail table in Power Query, right-click on it, and select “Disable load.” Once you do that, the menu will look like Figure 3-49. You’ll still be able to use the table in your merge, but it won’t be shown in the data model.