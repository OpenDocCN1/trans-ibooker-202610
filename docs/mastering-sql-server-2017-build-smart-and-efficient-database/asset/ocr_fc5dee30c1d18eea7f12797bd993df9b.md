# How to do it...

+   1. Add a new package to the AdventureWorksETL project. Rename it ProcessingExcel.dtsx.
+   2. Add a new package-level Excel connection manager. Rename it Excel_Ch11_Orders. Point to your Excel file path and define your Excel version appropriately, as shown in the following screenshot:
+   3. Add a Data Flow task to the control flow. Rename it to ProcessExcelOrders. Open the Data Flow tab.
+   4. Add an Excel source. Double-click it to open the editor.
+   5. Using the Connection Manager tab, define the Excel connection manager. Point to the Excel_Ch11_Orders connection manager you just created. Use the Table or view data access mode. Select Sheet1$ in the name of the Excel sheer drop-down list. Click on the Preview button to preview the content of the file, as shown in the following screenshot:

![](bbox=[0.125, 0.27, 0.875, 0.61])