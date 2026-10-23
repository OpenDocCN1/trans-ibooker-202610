+   - Queue: https://*.queue.core.windows.net
+   - Tables: https://*.table.core.windows.net
+   - Static Website: https://*.z6.web.core.windows.net

# FIGURE 9.39 Azure Storage account georeplication

![](bbox=[0.145, 0.225, 0.86, 0.615])

You can also perform the failover of a storage account using the following Azure CLI command:

```
az storage account show --name <accountName> --expand geoReplicationStats
az storage account failover --name <accountName>
```

Replace <accountName> with your Azure Storage account name. The first command will display the primary and secondary information as you saw previously in Figure 9.39. The second command will perform the actual failover process.

# Recovering Azure Messaging Services

There was not a lot about backup and recovery in Chapter 6 where the Azure Messaging services were covered in detail. From a conceptual perspective, in many ways the need for taking a backup doesn’t make a lot of sense. It doesn’t make a lot of sense because the data that is present in the queues wouldn’t remain there for long. The purpose of messaging services is to store a piece of information and to notify a consumer that information is present