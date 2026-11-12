```
await queue.AddMessageAsync(message);
}
```

Before we delve into an explanation of what this is doing, note that the string that's passed to `CloudStorageAccount.Parse()` is the connection string that we made a note of earlier. As you can see, I've put the connection string directly into the code. Needless to say, this is bad practice; however, as in previous chapters, I feel that it illustrates, the software that we are actually writing better, and more directly.

So, what is this code doing? Well, initially, we create a `CloudStorageAccount` instance; this effectively connects to the Azure storage account and acts as the basis for our future calls. Next, we're asking for a `CloudQueueClient` and then getting a reference to the queue itself using `GetQueueReference`. Finally, we build up a new message by simply serializing our class data and adding that to the queue.

A message should contain all the information that's necessary to deal with that message. You should think carefully about the size of your message; however, on storage queues, you have a limit of 200 GB!

# Testing our sales order generator and JMeter

Before we proceed, let's give our generator a quick test. There are two ways to do this; the first is to compile and run the software in the command line, but you can also simply provide a parameter in the project properties:

![](bbox=[0.105, 0.605, 0.895, 0.785])

As you can see, I've chosen to create a single order. It's always best to try creating a single item first, just to check it runs correctly. Once you know this works, you can up it to, say, 10 or 20.