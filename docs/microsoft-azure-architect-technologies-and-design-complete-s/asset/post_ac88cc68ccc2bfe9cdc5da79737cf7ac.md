
```
static CsharpGuitarHttpClientController()
{
    httpClient = new HttpClient();
}
public async Task<Product> GetTheCoolestGuitarEver(string id)
{
    ...
}
}
```

When you do this, the same connection object is used for all outbound connections to the same server. The pattern is the same when you are making connections to an Azure SQL database, an Azure Cosmos DB, or any system that requires the instantiation of a connection object.

## An Introduction to Coding for the Cloud

Throughout the book there have been references to Chapter 7 as the place where you will see how to actually implement some of the concepts. In this section, you will see those examples or get some tips on how to proceed with them. The following examples will be provided:

- Triggering a background job
- Connecting to regional/global database instances
- Working with the Azure Queue Storage SDK
- Using Forms Authentication, certificates, Windows Authentication, MFA, Open Standards, managed identities, and service principle authentication
- Reading encrypted data from a database

### Triggering a Background Job

A background job is a snippet of code that typically require high CPU, high memory, or long-running activities. Really, code snippets do anything that takes too long to trigger the background job and then wait for a response in real time. There are two primary ways in which a background job can be triggered: by an event or by a schedule. In the context of a WebJob, for example, an event trigger calls its exposed REST API. As you may recall, WebJobs are background job processors that run in the Azure App Service (PaaS) context. Assume you have an Azure App Service named csharpguitar that has the following endpoint:

```
csharpguitar.azurewebsites.net
```

A WebJob can be triggered using a similar endpoint. That endpoint would be similar to the following:

```
https://csharp guitar.scm.azurewebsites.net/api/triggeredwebjobs/
<webjobName>/run
```

These can be called from a browser manually or from another application that uses the HttpClient class. The other option is the use a scheduler like a CRON job that will trigger the execution of a batch job or a WebJob at certain intervals.

### Connecting to Regional/Global Database Instances

In Chapter 5 you learned about the global replication capabilities of the Azure Cosmos DB. The question was, how can you make code intelligent enough to connect to the read-only instances closest to the location of the client? A class in the Azure Cosmos DB SDK named ConnectionPolicy exposes a property called PreferredLocations. Preferred locations are used when creating the client that is used to make the connection to the database. Code within the SDK itself chooses the most optimal location. That is very cool. The snippet of code that achieves this is provided here:

```
ConnectionPolicy connectionPolicy = new ConnectionPolicy();
connectionPolicy.PreferredLocations.Add(LocationNames.SouthCentralUS);
connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope);
DocumentClient docClient =
    new DocumentClient(accountEndPoint, accountKey, connectionPolicy);
```

### Working with the Azure Queue Storage SDK

Some principles that are important to know about Azure Queue Storage are related to these three methods: PeekMessage(), GetMessage(), and DeleteMessage(). These methods, as well as the client code required to utilize them, are downloadable as a NuGet package from here:

```
www.nuget.org/packages/Microsoft.Azure.Storage.Queue
```

Installation instructions for Visual Studio are also available at that URL.

The PeekMessage() method is called with the following code snippet. Please note that there is code required before and after this snippet to make it work. For example, you must instantiate the storage account, instantiate the storage client, and identify the storage queue. This example/pseudocode is useful only for the explanation of the following implementation concept.

```
CloudQueueMessage peekedMessage = queue.PeekMessage();
```

Here is a snippet of code that calls the GetMessage() method and its asynchronous alternative GetMessageAsync():

```
CloudQueueMessage message = queue.GetMessage();
CloudQueueMessage retrievedMessage = await queue.GetMessageAsync();
```

The difference between the PeekMessage() and GetMessage() methods is that the GetMessage() method, when called, blocks the thread and waits until a message arrives in the queue before returning. The thread will be hung in a waiting state, which may not be a huge issue since there wouldn’t be any messages to process if the method call had to wait. Nonetheless, it is always good practice to release and not block threads in most, if not all, scenarios.

The benefit that PeekMessage() provides is that, when called, it tells you if there is a message in the queue waiting to be processed without blocking the thread or waiting on a message to arrive. Therefore, it is not necessary to call GetMessage() unless PeekMessage() returns with a status stating there is a message available for processing. PeekMessage() does not set any locks on the message and does not modify it in any way.

You may also consider calling the GetMessageAsync() method that would release the thread, but it would wait via await until there is a message in the queue ready for processing. Which process to use is up to the application requirements. One thing to note about using asynchronous methods is that all methods that include the await keyword must be asynchronous themselves (all the way up). It is not recommended to ever call Result() or Wait() in the async classes. This can cause serious blocking and hangs.

After one of the get message methods is called from your application, you need to call DeleteMessage() so that the queue knows the message has been processed. Notice that there is also an asynchronous version of this method and that it doesn’t return a value; it will, however, return a catchable exception if the deletion fails.

```
queue.DeleteMessage(retrievedMessage);
await queue.DeleteMessageAsync(retrievedMessage);
```

If there is an exception in your code and the message is not processed, then at some point the message would become unlocked and available for processing once again. The only way in which the queue knows that the message has been successfully processed is by you calling the DeleteMessage() method from your code after the processing is complete. You should seriously consider placing your code within a try{} ... catch{} block and in the catch expression perform some code to attempt a reprocess, place the message into a poison queue, log it for sure, and decide what you want to do with it later. It is possible that the message is malformed and would fail over and over again, so simply reprocessing the message isn’t the most optimal unless you have good logic in your catch expressions and handle them appropriately.

## Forms, Certificate, Windows, MFA, Open Standard, Managed Identities, and Service Principle Authentication

There are numerous methods for implementing authentication into your application. Some have been covered in detail; others have not. We will briefly cover several types, starting with Forms Authentication.

### Forms Authentication

Forms Authentication is implemented into an ASP.NET application starting with the installation of the IIS module. Then, its configuration is installed into the web.config file using syntax similar to the following snippet:

```
<configuration>
    <system.web>
        <authentication mode="Forms" />
    </system.web>
</configuration>
```

Figure 7.3 shows that the IIS module for Forms Authentication is installed but not yet enabled.

# FIGURE 7.3 IIS authentication providers

![](img/95043b3df926bcdadadb082b49262037_567_0.png)

Enabling the IIS module and the web.config file results in the authentication provider being enabled. The scope of this configuration is for the website running within and underneath the directory structure in direct relation to the location of the physical web.config file.

The reason for the note about where the configuration is applied in relation to the location of the web.config file is that this authentication provider can be enabled on the web server, but it’s not used unless you enabled it specifically on a website. In addition to configuring web.config, you must also have an Authentication (AuthN), in which your custom user identity database stores the credentials. Any group associations that define whether the user has access to specific resources within the application once authenticated, would utilize Authorization (AuthZ) to confirm the access. The code required to perform the AuthN and AuthZ procedure must be written by a developer working in the application itself. This is not an “out-of-the-box” authentication solution. It is, however, a solid, sophisticated, and valid approach for implementing security into an internet application.
