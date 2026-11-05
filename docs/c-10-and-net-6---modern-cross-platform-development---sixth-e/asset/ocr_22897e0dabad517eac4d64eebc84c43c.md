10. In the Object Explorer toolbar, click the Disconnect button.
11. Exit SQL Server Management Studio.

# Managing the Northwind sample database with Server Explorer

We did not have to use SQL Server Management Studio to execute the database script. We can also use tools in Visual Studio including the SQL Server Object Explorer and Server Explorer:

+   1. In Visual Studio, choose View | Server Explorer.
+   2. In the Server Explorer window, right-click Data Connections and choose Add Connection....
+   3. If you see the Choose Data Source dialog, as shown in Figure 10.3, select Microsoft SQL Server and then click Continue:

![](bbox=[0.18, 0.37, 0.93, 0.68])

Figure 10.3: Choosing SQL Server as the data source

+   4. In the Add Connection dialog, enter the server name as ., enter the database name as Northwind, and then click OK.
+   5. In Server Explorer, expand the data connection and its tables. You should see 13 tables, including the Categories and Products tables.
+   6. Right-click the Products table, choose Show Table Data, and note the 77 rows of products are returned.
+   7. To see the details of the Products table columns and types, right-click Products and choose Open Table Definition, or double-click the table in Server Explorer.