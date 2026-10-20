It returns a table with metadata for each table in your model, including the table name, whether it’s hidden, the storage mode, and whether it was created with a DAX expression.

Performing this overview is a great way to quickly assess your model’s structure. It can help you spot hidden tables that are no longer needed and tables that are built with calculated expressions that you can optimize. It also helps with cleanup, performance tuning, and even governance. Figure 6-2 shows the results of such a query.

![](bbox=[0.14, 0.35, 0.86, 0.57])

Figure 6-2. Results of an INFO.VIEW.TABLES() query in DAX query view

You can also retrieve information about the columns in our model with this code:

```
EVALUATE INFO.VIEW.COLUMNS()
```

This gives you an overview of all columns in your model, and it returns metadata like the column name, data type, and whether a column is hidden or not. This will help you review which columns are actually used, spot unnecessary columns still lingering in the model, and check consistency in naming and data types. Also, if your model is getting big, this kind of overview can save you a lot of clicking around.