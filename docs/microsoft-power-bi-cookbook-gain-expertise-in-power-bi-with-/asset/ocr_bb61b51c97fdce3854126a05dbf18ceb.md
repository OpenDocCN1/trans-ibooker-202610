+   19. Select a list item and edit the **Data** source to choose the table created previously (in this case, **DimProduct**)
+   20. For **Fields**, choose **Edit** and change **Title2** to be **Product Name**
+   21. Save the app

# See also

+   - Integrate Power BI data alerts with Power Automate: https://bit.ly/3tHLv5R
+   - Announcing the new Power Automate visual for Power BI: https://bit.ly/3vY9C1J
+   - Export and email a report with Power Automate: https://bit.ly/3fc8eBT
+   - Power Apps and Power BI, together at last: https://bit.ly/2QbQHkC
+   - Power Apps visual for Power BI: https://bit.ly/3yg5a0n

# Leveraging Dataverse and Dynamics 365

The Microsoft Power Platform includes Dataverse, what was formerly called the **Common Data Service (CDS)**. Dataverse stores business data securely in the cloud and allows you to manage the data used by business applications. Dataverse stores data in tables and includes base tables for common scenarios but also allows the creation of new, custom tables. Dynamics 365 applications such as Dynamics 365 Sales, Dynamics 365 Talent, and Dynamics 365 Customer Service store data natively in Dataverse and in addition, Power Apps can be used to build applications against this core data without the need for integration. Finally, Dynamics 365 Business Central and Dynamics 365 Finance and Operations also have native integrations with Dataverse.

This recipe demonstrates how to use Power BI with Dataverse and Dynamics 365.

# Getting ready

To prepare for this recipe, follow these steps:

+   1. If you have not already done so, configure a connection to your **AdventureWorksDW2019** database in your data gateway. See *Chapter 12, Deploying and Distributing Power BI Content* for details.
+   2. Open a web browser, go to https://admin.powerplatform.microsoft.com, and log on with your Microsoft 365 account. This is the Power Platform admin center.
+   3. In the left-hand navigation pane, click on **Environments.**