![](bbox=[0.243, 0.106, 0.757, 0.632])

+   9. Copy and paste the Azure Blob storage key in the `Cmgr_AzureStorage_ssiscookbook_AccountKey` parameter.
+   
+   10. From the SSIS toolbox-Azure section, drag and drop an Azure Blob upload task to the control flow. Set its properties as shown here:
+      - Rename the task `abut_test`
+      - Assign the **Connection** property to the `cmgr_AzureStorage_ssiscookbook` connection manager we created earlier.
+      - Create a folder on your C drive and name it `test`.
+      - Copy any file; in our case, we used a file called `ExtendedProperties.sql`.