# EXERCISE 5.3

# Creating an Azure SQL DB

+   1. Download a database backup by clicking the Download button located at this location:
+      github.com/benperk/ASA/blob/master/Chapter05/Ch05Ex03/guitar.bacpac

+   2. Log in to the Azure Portal at portal.azure.com, enter Storage Accounts in the search box at the top middle of the page, click Enter, select the Storage Account that you created in Exercise 5.1 (for example, csharpguitar), and click Containers (recall from Figure 5.11). Click the + Container button on the Containers blade, enter a Name (for example, guitar), select Blob from the Public access level from the drop-down, and then click the OK button.

+   3. Click the container that you just created then click the Upload menu item at the top of the Overview blade. Upload the .BACPAC which you downloaded in step 1.

+   4. Navigate to the SQL database server you created in Exercise 5.2 (for example, csharpguitar). Click the Import database menu item on the SQL database server Overview blade. Select the subscription into which the database will be imported into then select the Storage that contains the .BACPAC you created in the previous step. You should see something similar to Figure 5.24. Click the select button.

# FIGURE 5.24 Selecting a SQL database backup from an Azure Blob container

![](bbox=[0.148, 0.495, 0.863, 0.702])

+   5. Change the pricing tier to Basic, and provide a Server admin login ID and the password which you used in Exercise 5.2. Click the OK button.