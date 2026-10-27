As automatic detection is a **CURRENT FILE** option only, and since the setting is enabled by default, you currently need to disable this automatic type detection for each new file. It is recommended that you disable this for each file or manually remove the **Changed Type** step and make your own decisions regarding data type transformations.

Power Query also supports different locales or cultures. If there's any potential for **Date** data types to be sourced from a region with a different date standard than your local system, you should apply the **Locale** option to the Table.TransformColumnTypes function; for example, when a date column stores date values in the format dd/mm/yyyy, whereas the local system uses mm/dd/yyyy. Trying to convert this date directly on a desktop configured for US English causes an error as the first two digits are greater than 12. However, specifying the source locale in the transform expression allows for successful conversion. The Query Editor provides a simple interface for the source locale. Simply right-click on the column, expand **Change Type**, and then select **Using Locale....**

![](bbox=[0.12, 0.28, 0.85, 0.52])

Figure 2.40: Change Type with Locale interface

Alternatively, you can add the locale to the expression itself directly:

```
DateLocale =
    Table.TransformColumnTypes(
        PreviousStep, {{ "Date", type date }}, "en-GB"
    )
```

# See also

+   - Table.AddColumn: http://bit.ly/3vGJZ6b
+   - Table.TransformColumnTypes: http://bit.ly/3s5AfjC
+   - Data types in Power Query: http://bit.ly/392iWIP