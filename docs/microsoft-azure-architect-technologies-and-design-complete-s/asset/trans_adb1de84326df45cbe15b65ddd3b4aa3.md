

of the processing would need to be checked from time to time from the client, and once completed, the status check would respond with the results and a 200. In modern applications where web sockets are supported, the client and server can maintain a connection that allows the server to notify the client directly when the asynchronous request has completed successfully. This pattern is useful in legacy scenarios, scenarios that require HTTP, or where callbacks are not supported due to firewall restrictions on the client side.

## Queue-Based Load Leveling

Queue-based load leveling is the pattern useful for systems that may experience bursts or carry heavy loads that may overload some backend system such as a data store like Azure SQL. This overload could cause orders from an online store to time out if the load is too high for the backend servers to respond in an appropriate time. See the left portion of Figure 6.17, which illustrates numerous clients, an Azure App Service, and an Azure Function all attempting to update data on an Azure SQL database. The dotted line represents a failure of an update because the data store is overloaded.

### FIGURE 6.17 Queue-based load leveling messaging pattern

![](img/95043b3df926bcdadadb082b49262037_530_0.png)

The illustration on the right side of Figure 6.17 represents the queue-based load leveling pattern. This pattern places a message queue, for example, Service Bus or Azure Storage Queue, between the clients and the database. Then there is an Azure Function, which monitors that queue and processes the messages, perhaps sequentially, in near real time or at a scheduled interval. In any case, the dotted line representing a failure due to the overloading of the backend data store is not present. This is because the orders are queued up instead of all attempting to be processed and stored at the same time. This is a one-way communication; the client will not automatically receive notification that the order has been processed. This is something you would need to build into your business logic.

## Sequential Convoy

Many IT solutions require that transactions are processed in the order that they are received. At first glance, this may appear to be straightforward, because usually when you have a queue it is sequential, like when standing in a line; it's first come, first served. The complexity arises when there are multiple consumers processing the messages. What happens if consumer A for some reason can process faster than consumer B? Perhaps consumer A processes message 1, and consumer B processes message 2 but it hangs a little, and consumer A takes message 3 from the queue and completes it before message 2 is complete. Also consider a scenario where consumers can pull and process batches of messages at a time instead of one message at a time. What if multiple consumers can pull batches? It gets complicated, agreed? The solution is to implement a pattern similar to that shown in Figure 6.18, which is a visual representation of a sequential pattern.

### FIGURE 6.18 Sequential convoy load leveling messaging pattern

![](img/95043b3df926bcdadadb082b49262037_531_0.png)

The trick, or the expectation, is that the messages being sent to the queue can be categorized into groups of messages. Take, for example, when you have an order management system that supports the creation, update, and deletion of orders and assumes that, in a scenario, all three of those happen in a close sequence. If you send all order creations to category 0, which are processed by Consumer A, all updates to category 1, which are processed by Consumer L, and all deletes to category 3, which are processed by Consumer R, and the orders are processing in the order in which they are received, there is then no way that the system could ever attempt to delete an order that hasn’t yet been created.

## Competing Consumers

This pattern can probably be visualized by reading its name. A consumer is a VM that has some code on it, whose purpose is to process messages entering a queue. It is certainly possible to have a single publisher that sends messages to a queue that are then processed by a single consumer. However, in larger solutions, there are multiple publishers and/or clients that are putting messages into the queue, and the number of messages may be too much for a single consumer to process. This would lead to a large backlog in the queue, and it is possible that the consumer never gets caught up. It may take hours or days to process all the events. If that happens, you can provision multiple consumers that pull messages from the queue and process them in parallel. Figure 6.19 illustrates that scenario and represents the competing consumers’ messaging pattern.

## ARIMA 模型详解

ARIMA（自回归积分滑动平均模型）是时间序列分析中应用最为广泛的模型之一。其核心思想是将非平稳时间序列通过差分转化为平稳序列，再建立自回归（AR）和移动平均（MA）模型。

ARIMA 模型由三个参数组成：(p, d, q)。其中 p 为自回归项阶数，d 为差分阶数，q 为移动平均项阶数。模型的数学表达形式为：

```
Y_t = c + φ1*Y_{t-1} + φ2*Y_{t-2} + ... + φp*Y_{t-p}
      + ε_t + θ1*ε_{t-1} + θ2*ε_{t-2} + ... + θq*ε_{t-q}
```

在实际建模过程中，首先需要对时间序列进行平稳性检验（如 ADF 检验），若序列非平稳，则需进行 d 阶差分使其平稳。随后通过 ACF（自相关函数）和 PACF（偏自相关函数）图来确定 p 和 q 的取值。

- **AR（自回归）**：使用过去观测值与当前值的线性关系进行预测
- **I（差分）**：通过差分使非平稳序列变得平稳
- **MA（移动平均）**：使用过去预测误差的线性组合来修正预测

模型选择通常采用 AIC（赤池信息准则）或 BIC（贝叶斯信息准则）进行判断，值越小表明模型拟合效果越好。

## Azure Cache for Redis

In Chapter 5, where you learned about Azure Cache for Redis, you may recall one of the bullet points was its ability to act as a queue. This is more of a “for your information.” You can implement queuing and Pub/Sub messaging patterns with this technology. It would require some architectural configuration as well as custom code to implement. If this interests you, read about this capability at redis.io/topics/pubsub.

## Event Hubs

If you need to ingest huge amounts of data in a short period of time from sources like IoT devices, anomaly detection algorithms, telemetry streamers, or big data generators, then consider using Event Hubs. This messaging service is capable of ingesting millions of events per second, which is huge. The most common pattern to be implemented using Event Hubs is the Pub/Sub messaging pattern. Take a look at Figure 6.20, and you will see the resemblance of the Pub/Sub pattern illustrated earlier in Figure 6.16.

### FIGURE 6.20 Event Hub architecture components

![](img/95043b3df926bcdadadb082b49262037_533_0.png)

In Figure 6.20 you will find a visualization of the Event Hub architecture. There are numerous components that make up the architecture, for example the event producers (aka producers), which are depicted in the figure as IoT devices but can be anything that emits data. Your numerous partitions, multiple consumer groups, and event receivers (aka consumers) are also visualized. An event receiver can be any client that has subscribed in some way to get the Event Hub notification. Figure 6.20 shows Azure Functions as the event receivers, and the fact that there are numerous Azure Functions means that there can be multiple instances of an Azure Function, which can process events for a given consumer group and partition in parallel. Let’s explain some of the terminology described in this paragraph a bit more, beginning with a namespace. In Exercise 5.8 you created an Event Hub namespace; the name you gave it is considered the namespace and is the unique portion of the Event Hub endpoint, for example, <namespace>.servicebus.windows.net. View an Event Hub namespace as a container that contains one or more Event Hubs and where you configure security and monitoring features, which is similar to an Azure Storage account. In Exercise 6.4, you will create an Event Hub and two consumer groups.

## EXERCISE 6.4

### Creating an Event Hub and Two Consumer Groups

- 1. Log in to the Azure Portal at portal.azure.com. In the search box at the top middle of the browser, enter Event Hubs. Select Event Hubs and then select the Event Hub Namespace you created in Exercise 5.8. On the navigation menu, select Event Hubs and then click the + Event Hub link.
- 2. Enter an event hub name (for example, csharpguitar). Be sure the name you select is globally unique. Change the partition count to 3 and then click the Create button. Once it’s provisioned, select it from the Event Hub list. Click the Consumer Groups link from the navigation menu. Notice that a default consumer group named $Default already exists. Click the + Consumer Group link, enter a consumer group name (for example, guitar), and then click the Create button. (Note that the Basic tier supports only a single consumer group.)

An Event Hub can have up to 32 partitions; a partition is the location the producers stream the data into for queuing and storage. Since the ingress of data into the Event Hub is expected to be huge, this is how the load is balanced and queued at such a high rate, meaning the incoming data is stored across the numerous provisioned partitions. If you have multiple partitions, recognize that there is no guarantee onto which partition the message is stored, nor is the order of receipt retained. It is possible to send the partition ID as part of the ingress that would force it to that specific partition; however, this is not recommended. Take a look now at Consumer Groups. Notice there are three shown in Figure 6.20, and the maximum is 20. Also notice the different patterns of lines connecting the consumers to the partitions. This means that each consumer connected to their given consumer group will have the ability to read the data stored in the partitions. You would not want to have consumers bound to different consumer groups processing events that will be stored in the same backend system. This would result in duplicates, because each consumer group gets access to all the partitions in the Event Hub. When an event or batch of events are consumed, they are not deleted from the partition. Instead, the consumer marks it as committed by using offsets or a process called checkpointing. An offset is like a pointer or a cursor that identifies where in the data stream your consumers are at in terms of differentiating what has and what has not been processed. Checkpointing is the responsibility of the consumer to notify the messaging service that the stream on that partition is no longer needed for the consumers bound to a given consumer group.

Complete the following exercise to configure ADX to ingest Event Hub information, add messages to the Event Hub you created in Exercise 6.4, and query ADX to view the logs.

