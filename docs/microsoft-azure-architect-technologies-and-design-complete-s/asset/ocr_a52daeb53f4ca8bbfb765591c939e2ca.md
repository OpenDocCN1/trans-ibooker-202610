(The region you select should match the region of the resource group for consistency.) Select the Compute specifications. (I chose Dev(No SLA). ..it's the cheapest.) Click the Review + Create button then click the Create button.

+   2. Once the provision has completed, use the Overview blade and navigate to the cluster. Click the Create Database button, enter a database name (like csharpguitar), and leave the other settings at the defaults. Click the Create button.

+   3. In a browser, enter the following URL: dataexplorer.azure.com. Click the Add Cluster button. Enter the URL of the Azure Data Explorer Cluster that you just created and click the Add button. The URL will resemble the following HTTPS address, substituting the name you provided as the cluster name in step 1 (csharpguitar, in the example URL). Now check the region entry (southcentralus in the example). Change the region name to the region in which you created the cluster, if necessary. You will find the value on the Overview tab for the Azure Data Explorer Cluster.

```
csharpguitar.southcentralus.kusto.windows.net
```

+   4. Enter the following KUSTO query command .show databases, and click the Run button. The output should resemble that shown in Figure 5.49.

# FIGURE 5.49 Azure Data Explorer clusters in the ADX Web user interface

![](bbox=[0.15, 0.48, 0.86, 0.78])

+   5. The following KUSTO queries can be downloaded from here:

github.com/benperk/ASA/tree/master/Chapter05/Ch05Ex07

Create a table in the database by executing the next KUSTO command. Make sure your database is selected (in focus) in the ADX Web UI before clicking the Run button.

```
.create
table
```