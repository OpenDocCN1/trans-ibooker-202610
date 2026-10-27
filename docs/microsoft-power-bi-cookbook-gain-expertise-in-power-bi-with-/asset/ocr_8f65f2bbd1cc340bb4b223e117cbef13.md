# # How to provide documentation

To implement this recipe, perform the following steps:

+   1. Create a new blank M query called **TablesDMV** that retrieves the TMSCHEMA_TABLES DMV via the **Server** and **Database** parameters:

![](bbox=[0.201, 0.198, 0.866, 0.281])

Figure 10.8: Table metadata of the running Power BI Desktop file

+   2. Duplicate the **TablesDMV** query to retrieve the following five schema DMVs as well:
+   
+   - SYSTEM.TMSCHEMA_COLUMNS
+   - SYSTEM.TMSCHEMA_MEASURES
+   - SYSTEM.TMSCHEMA_ROLES
+   - SYSTEM.TMSCHEMA_TABLE_PERMISSIONS
+   - SYSTEM.TMSCHEMA_RELATIONSHIPS:

+   3. Name the queries according to their source, and organize these and their parameters into their own folders:

![](bbox=[0.2, 0.513, 0.867, 0.752])

Figure 10.9: Parameters and DMV queries used to support model documentation

+   4. Create a third query group named **Documentation** and a new blank query named **Columns**:

```
```m
let
    Tables = TablesDMV, Columns = ColumnsDMV,
    Join = Table.NestedJoin(Columns,{"TableID"},Tables,{"ID"},
```
```