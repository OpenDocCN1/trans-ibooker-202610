from any client other than resources within the VNet. To support an Azure SQL database service endpoint, you need to enable the Microsoft.Sql service and then configure the restriction on the Azure SQL database; this is the same process as with the Azure App Service. Complete Exercise 5.4, which walks you through the setup and testing of a service endpoint for an Azure SQL database.

# EXERCISE 5.4

# Creating a Service Endpoint for an Azure SQL Database

+   1. Open a cmd window on your local workstation and make a connection to the Azure SQL database you created in the previous exercise using SQLCMD similar to the following. The output should be similar to Figure 5.32. Refer to Table 3.11, which describes the requirements for installing SQLCMD. If required, use the connection properties you used when connecting via SSMS; see Figure 5.26. Replace * with the database server name and update <dbname>, <uid>, and <password> to the ones you provided.

```
sqlcmd -S *.database.windows.net -d <dbname> -U <uid> -P <password>
select database_id, is_read_only, compatibility_level from sys.databases
go
```

# FIGURE 5.32 Connecting to an Azure SQL database using SQLCMD from an Azure VM

![](bbox=[0.18, 0.505, 0.81, 0.67])

+   2. Make a remote connection (RDP) to an Azure VM you created earlier. Refer to Exercise 3.3 if you need to create a new one. Attempt the same SQLCMD connection to the Azure SQL database you created in the previous exercise using SQLCMD. The output will be similar to Figure 5.32.

# Firewall Settings

For step 1 to work, your client IP must be added to the white-list for the Azure SQL database. For the connection in step 2 to work, the setting Allow Azure Services And Resources To Access This Server must be enabled, set to ON.

+   3. Navigate to the Azure VNet that you created in Exercise 3.2 (for example, CSHARPGUITAR-VNet-B) or create a new one. The VM in step 2 must be located in