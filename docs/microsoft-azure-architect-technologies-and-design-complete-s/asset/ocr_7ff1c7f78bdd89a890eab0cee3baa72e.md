# Other Azure Data Stores

Azure SQL and Azure Cosmos DB are by far the most commonly provisioned data stores on Azure and if there are any questions on the Azure Solution Architect Expert exam, they would be about those two. But to become the best Azure Solution Architect Expert possible, you should know some about other data store offerings on Azure.

# Table Storage

If you return to Figure 5.11, you will see that Azure Table Storage is contained within an Azure Storage account, just like Azure Files, Queues, and Blob containers. In short, an Azure Table is a place where you can store data in a nonstructured way based on key/value pairs. The ability to store petabytes of data and manage the scaling are the major benefits available for this offering. Both the compute and storage requirements are managed by the Azure platform. When creating the storage account, you decide which redundancy option is required, for example, LRS, ZRS, GRS, and so forth, which is helpful for availability and DR scenarios.

Table Storage is based on two fundamental concepts, the partition key and the row key. Once the table itself is created, either using the portal or, for example, Azure PowerShell, you can configure both of them. After importing the AzTable module using Import-Module AzTable, when you add data to the table, both the partition key and row key are required. An Azure PowerShell command similar to the following would insert a row into the Azure Table:

```
Add-AzTableRow -table $tableName -partitionKey $pk -rowKey ("FEN1") /
    -property @{"MODEL"="TELECASTER";"LOCATION"="USA"}
```

Figure 5.42 provides a visualization of the Azure table partitioning concept. You will notice the Azure Storage account with two tables. The table can have multiple partition keys that then define the physical partitions created on the disk. This is similar to the sharding technique discussed in the context of numerous other data store products up to now. However, in the case of Azure Tables, the partitioning is done for you, by the platform, so long as you add the data to the table in the appropriate form, i.e., table, partition key, row key, data.

# FIGURE 5.42 Azure Table storage partitioning

![](bbox=[0.13, 0.79, 0.88, 0.94])