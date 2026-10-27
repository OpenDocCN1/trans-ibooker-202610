# How to do it...

To implement this recipe, use the following steps:

+   1. Navigate to https://powerapps.com and switch to the new environment created in *Getting ready*. The current environment is located in the upper-right corner.
+   2. Expand **Data** and choose **Connections**.
+   3. Click **New connection**.
+   4. Search for **Dynamics** and add a **Fin & Ops Apps (Dynamics 365)** connection.
+   5. Repeat *Steps 3 and 4* to add a **Dataverse** connection and a **Dynamics 365 (deprecated)** connection.
+   6. Under the **Data** heading, now choose **Tables**.
+   7. Switch to the **All** view and search for *warehouse*.
+   8. Click the **Warehouse** table.
+   9. Click **Add column**.
+   10. Add a **City** text column.
+   11. Click **Save Table** in the lower-right corner.
+   12. Navigate to the Power Platform admin center.
+   13. Click on **Data integration**.
+   14. Click **New connection set**.
+   15. Provide a descriptive value for **Connection set name**.
+   16. For the **First app connection**, choose your **Finance and Operations apps** connection created previously and select the correct environment.
+   17. For the **Second app connection**, choose **Dynamics 365 for Sales** and select the environment created in this recipe. Note that at the time of writing, **Dynamics 365 for Sales** is the correct connection type. The legacy **Common Data Service** connection type does not work and the new **Dataverse** connection type does not appear in the list of available applications. In the future, the **Dataverse** connection type will likely be the correct choice.
+   18. Select the correct organization, such as **USMF** for **Finance and Operations apps**, and the environment name for the **Dynamics 365 for Sales** organization.
+   19. Click the **Save** button.
+   20. Click **New project**.
+   21. Provide a **Project name** such as **Warehouse Sync**.
+   22. Click **Choose an existing connection set** and select the connection set created previously.