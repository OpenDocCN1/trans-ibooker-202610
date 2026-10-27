![](bbox=[0.2, 0.081, 0.867, 0.378])

The **Detected R IDEs** and **Detected Python IDEs** dropdowns can be used to choose between multiple installed IDEs. If R and Python script visuals have not been used in Power BI Desktop, an **Enable script visuals** prompt will appear. Click **Enable**.

+   6. Install matplotlib for Python using the following command: `python -m pip install matplotlib`.

+   7. Install pandas for Python using the following command: `python -m pip install pandas`.

+   8. Access the **Power Query Editor** by clicking on **Transform Data** in the ribbon of the **Home** tab.

+   9. Create a query named AdWorksDW similar to the following:

```
```powerquery
let
    Source = Sql.Database("localhost\\MSSQLSERVERDEV", "AdventureWorksDW2019")
in
    Source
```
```