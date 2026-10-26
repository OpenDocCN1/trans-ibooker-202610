# Creating a DQS knowledge base

A DQS KB is the place where you store the knowledge about the data and the cleansing in order to speed up the regular cleansing process. In a real-life scenario, you constantly add knowledge to the KB, and thus improve the cleansing process over time. In this recipe, you will create a basic DQS KB.

# Getting ready

For this recipe, you will need DQS and Data Quality Client installed. Please refer to the following link to learn how to install the DQS components:
https://docs.microsoft.com/en-us/sql/data-quality-services/install-windows/install-data-quality-services.

You also need to prepare the data you will use to create a DQS KB. In SSMS, execute the following code:

```
USE AdventureWorksDW2014;
SELECT DISTINCT
City, StateProvinceName AS StateProvince,
EnglishCountryRegionName AS CountryRegion
INTO dbo.AWCitiesStatesCountries
FROM dbo.DimGeography;
GO
```

# How to do it...

+   1. Open the Data Quality Client application and connect to your DQS instance.
+   2. In the Knowledge Base Management group, click the New KnowledgeBase button.
+   3. Name the database AWCustomers. If you want, add a description. Make sure that the None option is selected in the Create Knowledge Base from drop-down list. Select the Knowledge Discovery option in the Select Activity list in the lower-right corner of the screen. Click Next.
+   4. On the Map tab of the Knowledge Base Management screen, select SQL Server as your data source. Select the AdventureWorksDW2014 database and the dbo.AWCitiesStatesCountries table.