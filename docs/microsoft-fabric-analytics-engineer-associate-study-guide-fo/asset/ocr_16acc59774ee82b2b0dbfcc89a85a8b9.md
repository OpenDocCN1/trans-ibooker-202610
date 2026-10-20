+   c. Enable users to edit data models in the Power BI service from the tenant settings.
+   d. In the Tenant settings, set “Allow XMLA Endpoints” and “Analyze in Excel with on-premises semantic models” to Enabled.
+   e. In the Tenant settings, enable “Publish to Web.”

# 2. You’re creating a semantic model in Microsoft Power BI Desktop, and you’ve developed changes to a semantic model in the TMDL format. Which file format should you use for the Power BI Desktop file?

+   a. .pbip
+   b. .pbit
+   c. .pbix
+   d. .pbids

# 3. In a Fabric Warehouse, you have a table named FactSales that has one billion rows. You run the following T-SQL statement:

```
CREATE TABLE mirror.FactSales AS CLONE OF dbo.FactSales;
```

Select the statement or statements that are true.

+   a. A replica of dbo.FactSales is created in the mirror schema by copying the metadata only.
+   b. Additional schema changes to dbo.FactSales will also apply to mirror.FactSales.
+   c. Additional data changes to dbo.Factsales will not apply to mirror.FactSales.