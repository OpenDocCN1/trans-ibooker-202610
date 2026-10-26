# Create a database

This recipe, like many others in this book, requires a database to be created. It can be done by using SQL Server Management Studio. Start SSMS, connect to your local instance and right-click on the database folder. From the contextual menu, choose New Database.... The following window appears:

![](bbox=[0.135, 0.23, 0.865, 0.77])

Fill in the Database name and click OK as we don't need to change the Owner and Database files information. You should now see the database under the Databases folder in SSMS. We're now ready to create an SSIS package that'll use it.