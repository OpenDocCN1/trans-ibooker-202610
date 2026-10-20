![](bbox=[0.18, 0.09, 0.86, 0.19])

Figure 2-18. Destination list for individual tables in Dataflow Gen2

The ability to add custom destinations in Dataflow Gen2 is very useful. Now, we can save data where we want to, directly in the Fabric workspace item we require.

> **TIP**
> 
> Note that the Azure Data Explorer (Kusto) connectors from both the default destination and the data destination support the eventhouse KQL database from Fabric. Even though it’s not a part of the OneLake catalog in the selection of destinations, you can copy the URL from the KQL database and use it as a destination for your dataflow.

The rest of Dataflow Gen2 for development and coding capabilities is the same as described previously for Dataflow Gen1.

Now that we have covered both Dataflow Gen1 and Dataflow Gen2, it is a natural next step to create data connections with Power BI Desktop. In the next section, we’ll dive into the details of how to create data connections to Power BI, both from external sources outside Fabric and internal sources such as OneLake, semantic models, and KQL querysets.

# Data Connections from Power BI

When you work with data and want to build semantic models or visualizations, you have two options for getting started. On the one hand, you can use the Power BI