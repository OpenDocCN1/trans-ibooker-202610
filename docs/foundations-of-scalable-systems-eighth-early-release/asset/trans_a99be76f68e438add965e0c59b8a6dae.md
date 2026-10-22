

![](img/936a72197720a5158b16541313af61c9_185_0.png)

图 7-6. RabbitMQ 连接和通道

为了提高 RabbitMQ 客户端的吞吐量，一种常见策略是实现多线程生产者和消费者。然而，通道不是线程安全的，这意味着每个线程都需要对通道拥有独占访问权。如果你的客户端拥有长生命周期的有状态线程，并且可以为每个线程创建一个通道，则这不是问题，如图 7-6 所示。你启动一个线程，创建通道然后进行发布或消费。这是一种每个线程一个通道的模型。

在 Tomcat 或 Spring 等应用服务器中，解决方案却没那么简单。线程的生命周期和调用由服务器平台控制，而不是你的代码。解决方案是在服务器初始化时创建一个全局通道池。这个预先创建的通道集合可以在不产生每次请求创建和删除通道的开销的情况下，按需被服务器线程使用。每次请求到达处理时，服务器线程执行以下步骤：

- 从池中检索一个通道
- 将消息发送到代理
- 将通道归还池以供后续重用

虽然 RabbitMQ 没有原生能力来实现这一点，但在 Java 中你可以使用 Apache Commons Pool 库⁵ 来实现通道池。此实现的完整代码包含在本书的随附代码库中。以下代码片段展示了服务器线程如何使用 Apache GenericObjectPool⁶ 类的 borrowObject() 和 returnObject() 方法。你可以使用 setter 方法调整此对象池的最小和最大大小，以提供应用程序所需的吞吐量。

```
private boolean sendMessageToQueue(JsonObject message) {
    try {
        Channel channel = pool.borrowObject();
        channel.basicPublish(// arguments omitted for brevity)
        pool.returnObject(channel);
        return true;
    } catch (Exception e) {
        logger.info("Failed to send message to RabbitMQ");
        return false;
    }
}
```

在消费者端，客户端创建可用于接收消息的通道。消费者可以使用 basicGet() API 按需从队列中显式检索消息，如以下示例所示：

```
boolean autoAck = true;
GetResponse response = channel.basicGet(queueName, autoAck);
if (response == null) {
    // No message available. Decide what to do ...
} else {
    // process message
}
```

这种方法使用拉取模型，也称为轮询。轮询效率低下，因为它涉及忙等待，迫使消费者不断请求消息，即使没有消息可用。在高性能系统中，这不是应采用的方法。

替代且更优选的方法是推送模型。消费者指定一个回调函数，当 RabbitMQ 代理发送或推送消息给消费者时会被调用。消费者调用 basicConsume() API。当队列中有消息可供消费者使用时，消费者上的 RabbitMQ 客户端库会在与通道关联的另一个线程中调用回调。以下代码示例展示了如何使用传递给 basicConsume() 以建立连接的 DefaultConsumer 类型对象接收消息：

```
boolean autoAck = true;
channel.basicConsume(queueName, autoAck, "tag",
    new DefaultConsumer(channel) {
        @Override
        public void handleDelivery(String consumerTag,
                                    Envelope envelope,
                                    AMQP.BasicProperties
properties,
                                    byte[] body)
            throws IOException
        {
            // process the message
        }
    });
```

在单个通道上接收消息是单线程的。这使得有必要创建多个线程并分配每个线程一个通道或通道池，以便获得高消息消费速率。以下 Java 代码片段展示了如何实现这一点。每个线程创建并配置自己的通道，并指定当 RabbitMQ 客户端交付新消息时应调用的回调函数 – threadCallback()。

```
Runnable runnable = () -> {
    try {
        final Channel channel = connection.createChannel();
        channel.queueDeclare(QUEUE_NAME, true, false, false, null);
        // max one message per receiver

        final DeliverCallback threadCallback = (consumerTag,
 delivery)
            -> {
                String message =
                    new String(delivery.getBody(),
 StandardCharsets.UTF_8);
                // process the message
            };
        channel.basicConsume(QUEUE_NAME,
```

```
false, threadCallback, consumerTag ->
{});    //
} catch (IOException e) {
    logger.info(e.getMessage());
}
```

为了获得高性能和可扩展性，了解 RabbitMQ 使用的消息代理线程模型也很重要。在代理中，每个队列由单个线程管理。这意味着如果你的队列数量至少与底层节点上的核心数一样多，你可以在多核节点上提高吞吐量。反之，如果你的代理节点上高度利用的队列数量远多于核心数，你可能会看到性能下降。

像大多数消息代理一样，RabbitMQ 在消费速率跟上生产速率时表现最佳。当队列变长，达到数万条消息的量级时，管理队列的线程会产生更多开销。默认情况下，代理将使用其运行节点上 40% 的可用内存。当达到此限制时，代理将开始限制生产者，减慢代理接受消息的速率，直到内存使用量降至 40% 阈值以下。内存阈值是可配置的，再次，这是一个可以根据工作负载进行调整以优化消息吞吐量的设置。⁷

## 数据安全与性能权衡

所有消息系统都围绕性能与可靠性权衡呈现两难境地。在这种特定情况下，核心问题是消息传递的可靠性，通常称为数据安全。你希望消息在生产者与消费者之间以最小延迟传输，当然你也不希望在途中丢失任何消息。永远不要。记住，这些是分布式系统。

当消息从生产者传输到消费者时，你必须理解并在设计中考虑多种故障场景。这些是：

- 生产者向代理发送消息，但消息未被代理成功接受
- 消息在队列中，而代理崩溃
- 消息成功交付给消费者，但消费者在完全处理消息之前失败

如果你的应用程序可以容忍消息丢失，那么你可以选择最大化性能的选项。如果偶尔丢失即时消息应用程序中的消息可能无关紧要。在这种情况下，你的系统可以忽略消息安全问题并全速运行。这对于例如采购系统来说并非如此。如果采购订单丢失，企业会损失金钱和客户。你需要建立保障措施以确保数据安全。

RabbitMQ 与几乎所有消息代理一样，都有可用于保证端到端消息传递的功能。这些是：

### 发布者确认

发布者可以指定希望从代理接收确认，表明消息已成功接收。这不是默认的发布者行为，必须通过调用 `confirmSelect()` 方法将其设置为通道属性。发布者可以同步等待确认，或通过注册回调函数异步等待。

### 持久化消息和消息队列

如果消息代理失败，则存储在每个队列内存中的所有消息都会丢失。为了在代理崩溃后存活，队列需要配置为持久化，或持久。 这意味着消息在从发布者到达后立即写入磁盘。当代理在崩溃后重新启动时，它会恢复所有持久队列和消息。在 RabbitMQ 中，队列和单个消息都需要配置为持久化，以提供高水平的数据安全。

