Now, we can run this image, just to check that it's built correctly. To do so, use the following command:

```
docker run salesorderprocess
```

You should be presented with the output of the console application; since we haven't changed anything yet, this is the default Hello World.

Now that we have successfully created the container, let's start by adding the logic that will make up the microservice.

# Creating the microservice logic

The logic itself is relatively straightforward; however, you'll notice some gaps in here where we are using methods and classes that either don't exist or are not accessible. We'll start by putting our logic into a separate class; to make the class easy to understand, we'll create a public function called Run:

```
public async Task Run()
{
    while (true)
    {
        if (!await ProcessEachMessage())
        {
            await Task.Delay(60000);
            continue;
        }
    }
}
```

The logic here is that we try to get the next message from the service bus; when we are not successful, we wait for a minute and try again. Let's see what ProcessEachMessage looks like:

```
public async Task<bool> ProcessEachMessage()
{
    SalesOrder.Models.SalesOrder? salesOrder = await
    _serviceBusHelper.GetNextOrderFromMessageQueue();

    if (salesOrder == null) return false;

    _salesOrderService.Create(salesOrder);
    await _serviceBusHelper.ConfirmSalesOrderToMessageQueue(salesOrder);
}
```