In this brief recipe a data model and its source M queries contained in an Excel workbook are migrated to a Power BI dataset via the Import Excel Workbook to Power BI Desktop migration feature. Additional details on the workbook content imported and other options and considerations for Excel-to-Power BI migrations are included in the *How it works...* and *There's more...* sections.

# Getting ready

Analyze the Excel workbook to identify the components that can be imported to Power BI Desktop. For example, a table or range of data in an Excel worksheet will not be imported but tables in the Excel data model will be imported. Similarly, Power View report sheets in Excel and their visuals will be migrated by standard Excel charts, pivot tables, and worksheet formulas and formatting will not be migrated.

In some scenarios it may be necessary to revise the Excel workbook to establish a data source connection and query that will be migrated. Additionally, it may be necessary to re-create Excel-specific report visualizations such as pivot tables and charts with Power BI Desktop report authoring visuals. Excel workbooks that contain a high level of customization such as VBA macros and complex Excel formula logic may require significant modifications to the Excel workbook or to the Power BI Desktop model or some combination of both to support a migration.

# How to do it...

To implement this recipe, do the following:

+   1. Save or download the latest Excel workbook
+   2. Open a new Power BI Desktop (PBIX) file
+   3. From **Report View**, click **File | Import | Power Query, Power Pivot, Power View**

![](bbox=[0.2, 0.638, 0.867, 0.839])

Figure 13.13: Import queries and models created in Excel to Power BI

+   4. Select the Excel file and click the **Open** button