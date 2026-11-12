I've left my connection string here to illustrate what the string would look like; obviously, by the time this book is published, the storage account will not exist anymore, so please replace this with your own connection string; otherwise, it will not connect. In truth, you can structure this file differently if you so wish; by looking at ConfigurationBuilder, you'll see that, providing it's in a key-value pair structure, it makes little difference.

Now, let's look at how we can extract that configuration. Inside AzureStorageClientService, we'll create a constructor:

```
private string _connectionString;
public AzureStorageClientService()
{
 var builder = new ConfigurationBuilder()
 .SetBasePath(Directory.GetCurrentDirectory())
 .AddJsonFile("appsettings.json");
 var configuration = builder.Build();
 _connectionString =
 configuration["ConnectionStrings:netcodephotostorage"];
}
```

All we're doing here is using ConfigurationBuilder to load and parse the JSON file that we've just created. Once done, we can use the resultant configuration property to extract key-value pairs.

Now, if we run the console application, we should be able to see that the file has been uploaded to the Azure Cloud Storage:

![](bbox=[0.102, 0.638, 0.895, 0.862])