+   6. Click the **OK** button to exit the **Manage Parameters** dialog.
+   7. Edit the AdWorksDW query to use the new parameters:

```
```powerquery
let
    SQLInstance =
        if #"SQL Server Instance" = null
        then "localhost\\MSSQLSERVERDEV" else #"SQL Server Instance",
    SQLDatabase =
        if #"SQL Database" = null
        then "AdventureWorksDW2019" else #"SQL Database",
    Source = Sql.Database(SQLInstance, SQLDatabase)
in
    Source
```
```

In this case, the modified AdWorksDW query demonstrates how to account for parameters that may not be required. The query uses if statements to determine whether the parameter value is null and, if so, uses a default value for the SQLInstance and SQLDatabase expressions. If saved as a template, these parameters for the SQL Server Instance and SQL Database would be presented to the user upon opening the PBIT file. This would allow the user to connect to their own local copy of the database to retrieve the data used in the report. The user can then save the file as a PBIX.

# See also

+   ➤ Using templates in Power BI Desktop: https://bit.ly/3gpXtxX

# Converting static queries to dynamic functions

In addition to the standard library of functions available to M queries, user-defined functions can be created to encapsulate the logic of queries for dynamic application against parameter inputs. Like SQL-stored procedures, M functions can be created with or without input parameters, which can be required or optional. Additionally, as functions are values in the M language, just like table and list values, they can be invoked on demand and in multiple areas within a given Power BI data model.

In this recipe, a function is created to support the integration of a list of employee IDs maintained outside the data warehouse environment. The function accepts the employee ID values as parameter inputs and retrieves related column values.