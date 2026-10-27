# Getting ready

To prepare for this recipe, follow these steps:

+   1. Open the Power BI Desktop file containing the dataset to be analyzed. This file must remain open during data retrieval.
+   2. Open DAX Studio from External Tools.
+   3. Retrieve the server name and port associated with the running Power BI Desktop file from the bottom right of the DAX Studio status bar; an example would be localhost:56514.
+   4. In DAX Studio, in the upper-left corner under Metadata, right-click the database ID and choose Copy Database ID.
+   5. Open a new Power BI Desktop file and click on Transform data in the ribbon of the Home tab to open the Power Query Editor window.
+   6. Create two parameters, Server and Database, and apply the values retrieved from DAX Studio as the current values:

![](bbox=[0.2, 0.4, 0.87, 0.5])

Figure 10.4: Query Parameters with values for the local Power BI model

# How to import and visualize DMV data

To implement this recipe, perform the following steps:

+   1. Create a Segments query:
+   ```
+   let Source = AnalysisServices.Database
+   (Server, Database,[Query="Select * From $SYSTEM.DISCOVER_STORAGE_TABLE_COLUMN_SEGMENTS"]),
+       Segments = Table.AddColumn(Source, "Structure Type", each
+           if Text.Range([TABLE_ID],1,1) <> "$" then "Data"
+           else if Text.Start([TABLE_ID],2) = "H$" then "Column Hierarchy"
+           else if Text.Start([TABLE_ID],2) = "U$" then "User Hierarchy"
+           else if Text.Start([TABLE_ID],2) = "R$" then "Relationship"
+           else "unknown", type text),
+       RenameTable = Table.RenameColumns(Segments,{{"DIMENSION_NAME", "Table"}}),
+       KeyColumn = Table.AddColumn(RenameTable, "ColumnKey", each [Table] & "-" & [COLUMN_ID], type text)
+   in KeyColumn
+   ```