# The storage queue

We have two methods that need to be completed from the storage queue. Our first method is ConfirmSalesOrderToMessageQueue; this is responsible for posting a copy of the sales order to a new queue to indicate that we have processed it. You may start to notice a similarity between this and the method we wrote earlier to send a message to the queue in the first place:

```
```csharp
public async Task ConfirmSalesOrderToMessageQueue (Models.SalesOrder
salesOrderData)
{
    CloudStorageAccount storageAccount =
    CloudStorageAccount.Parse ("DefaultEndpointsProtocol=https;AccountName=sales
orderqueue;AccountKey=aa8eED0s5wezSDCyj0BmukVq2zEp9uEFRVq4jIRr+n8L1NNSUyZ2
ajXZHVN91BgsQQ9sPE2gnlsb5MWC1TsVw==;EndpointSuffix=core.windows.net");

    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient ();
    CloudQueue queue = queueClient.GetQueueReference ("salesorderconfirm");
    await queue.CreateIfNotExistsAsync ();
    CloudQueueMessage message = new
    CloudQueueMessage (JsonConvert.SerializeObject (salesOrderData));

    await queue.AddMessageAsync (message);
}
```
```

> Remember to replace the connection details with your own (these are not real account details).

In fact, the two methods are almost identical. The queue name is different, but clearly, the two methods could be refactored to be one, should you wish to do so. For now, I'm going to leave them separate as I think it illustrates their different functions better; however, I won't reexplain the code.

Finally, we have the code to get the next sales order from the queue, that
is, GetNextOrderFromMessageQueue:

```
```csharp
public async Task<Models.SalesOrder?> GetNextOrderFromMessageQueue ()
{
    CloudStorageAccount storageAccount =
    CloudStorageAccount.Parse ("DefaultEndpointsProtocol=https;AccountName=sales
orderqueue;AccountKey=aa8eED0s5wezSDCyj0BmukVq2zEp9uEFRVq4jIRr+n8L1NNSUyZ2
ajXZHVN91BgsQQ9sPE2gnlsb5MWC1TsVw==;EndpointSuffix=core.windows.net");
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient ();
    CloudQueue queue = queueClient.GetQueueReference ("salesorder");
```
```