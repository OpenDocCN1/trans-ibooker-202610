# EXERCISE 6.5

# Configuring ADX to Receive

+   1. Log in to the Azure Portal at portal.azure.com. In the search box at the top middle of the browser, enter Azure Data Explorer Clusters and then select Azure Data Explorer Clusters. Select the Azure Data Explorer Cluster you created in Exercise 5.7. On the navigation menu, select Databases and then click the database you created in Exercise 5.7. Click the Data Ingestion link on the navigation menu and click the + Add Data Connection link.
+   2. Select Event Hub from the Select Connection Type drop-down list. Enter a name in the Data Connection Name text box (for example, csharpguitar-conn). Select the subscription in which the Event Hub exists and then select the Event Hub namespace you created in Exercise 5.8. Select the Event Hub you created in Exercise 6.4 and select $Default from the Consumer Group drop-down list. Select None from the Compression drop-down list.
+   3. Add the ADX table name that you created in Exercise 5.7 step 5 (for example, CsharpGuitarEventHub) and then select JSON from the Data Format drop-down list. Enter the Column mapping name that you created in Exercise 5.7 step 6 (for example, CsharpGuitarEventHubMapping). The final configuration should resemble that shown in Figure 6.21. Now click the Create button.

# FIGURE 6.21 ADX to Event Hub Data connection configuration

![](bbox=[0.278, 0.545, 0.71, 0.825])