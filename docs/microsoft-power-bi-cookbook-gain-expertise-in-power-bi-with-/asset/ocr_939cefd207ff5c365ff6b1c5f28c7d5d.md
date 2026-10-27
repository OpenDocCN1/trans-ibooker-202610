# There's more...

Using Import mode, we can do many things to enhance our queries to aid in report development and display. One such example is that we can add additional columns to provide automatic sorting of an attribute in report visuals. Specifically, suppose that we wish for the United States regional organizations to appear next to one another by default in visualizations. By default, since the Organization column in the DimOrganization table in AdventureWorksDW2019 is a text column, the Central Division (a part of the USA), appears between Canada and France based upon the default alphabetical sorting of text columns. We can modify a simple query that pulls the DimOrganization table to add a numeric sorting column. To see how this works, follow these steps:

+   1. Using the same Power BI file used for this recipe, open the Power Query Editor, right-click the AdWorksDW query, and select Reference.
+   2. Choose the DimOrganization table and rename the query to DimOrganization.
+   3. Open the Advanced Editor window for the DimOrganization query.
+   4. Add a Table.Sort expression to the import query for the DimOrganization dimension. The columns for the sort should be at the parent or higher level of the hierarchy.
+   5. Add an expression with the Table.AddIndexColumn function that will add a sequential integer based on the table sort applied in the previous step. The completed query should look something like the following:
+   6. Finally, with the Ctrl key pressed, select the OrganizationKey, OrganizationName, and OrgSortIndex columns by clicking their column headers. Right-click on the OrgSortIndex column and choose to Remove Other Columns. The preview data should now show as presented in Figure 2.24.

```
let
    Source = AdWorksDW,
    dbo_DimOrganization = Source{[Schema="dbo",Item="DimOrganization"]}[Data],
    OrgSorted = Table.Sort(
        dbo_DimOrganization,
        {
            {"ParentOrganizationKey", Order.Ascending},
            {"CurrencyKey", Order.Ascending}
        }
    ),
    OrgSortIndex = Table.AddIndexColumn(OrgSorted,"OrgSortIndex",1,1)
in
    OrgSortIndex
```