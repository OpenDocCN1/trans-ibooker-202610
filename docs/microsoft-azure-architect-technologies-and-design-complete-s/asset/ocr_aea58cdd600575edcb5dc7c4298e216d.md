Notice that Figure 6.25 depicts a single sender; however, the messages are placed into four different queues. Those queues are referred to as *subscriptions*. When you configure a receiver to read from a Service Bus topic, it is required to provide the subscription name. Each subscription will receive a copy of the message that can then be processed by the subscribed receiver. This scenario may remind you of the Sequential Convoy pattern, as shown in Figure 6.18.

A few additional attributes available with a Service Bus are *dead-letter queue (DLQ)*, batching, and Peek-Lock. A DLQ is a queue where the Service Bus places messages that cannot be delivered. It would be prudent for a system designer to build in some logic that checks this queue from time to time to see whether there are any messages present in the DLQ. The DLQ ensures that no messages are lost. You will need to code the analysis of these DLQ messages into your application and find out for yourself why they failed.

Batching is an efficient concept. Instead of receiving one message, processing it, getting another, and processing it, the receiver can get a batch of messages, perhaps based on a timestamp range, and process them all in a single invocation. This saves some time when compared to making a network call for each message.

The last attribute is an interesting one named Peek-Lock (a nondestructive read). Peek-Lock is an approach that helps prevent the processing of a message in a Service Bus queue multiple times. To achieve this, you instruct a receiver to place a Peek-Lock on a message that includes a timed duration for how long the lock is valid. If the lock duration expires, then the message is again available for other receivers for processing. The timed duration is helpful for managing exceptions or timeouts during the processing of a message; this makes sure that if the receiver cannot successfully process the message that it gets back into the queue and isn’t lost due to problems with the receiver or some resource the receiver relies on.

# Azure Storage Queue

There was a lot discussed about Azure Storage Queue in the previous chapter. When you create an Azure Storage Queue, you create it in the context of an Azure Storage account, as you would for an Azure blob, an Azure file, or an Azure table. The architecture components for the flow of messages through an Azure Storage queue is similar to that shown in Figure 6.24, and you have already read some differences between Service Bus and an Azure Storage queue in Table 6.1. As you may recall from Table 6.1, the Service Bus and Azure Storage queues are message-based, but Service Bus has more capabilities like topics and FIFO. Some additional differences will make the decision on which one to use more straightforward. You will find them in Table 6.2.

**TABLE 6.2**  Comparing Azure Storage Queue and Service Bus

|  | Azure Storage Queue | Service Bus |
|---|---|---|
| Ordering | No | Yes |
| Atomic | No | Yes |