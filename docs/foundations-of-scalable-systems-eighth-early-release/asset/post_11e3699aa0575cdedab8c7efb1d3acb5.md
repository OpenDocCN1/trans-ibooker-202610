
a topic. Each event published on the topic is then delivered to all interested consumer services. I’ll return to event-driven architectures² when microservices are covered in Chapter 9.

## Message Replication

In an asynchronous system, the message broker is potentially a single point of failure. A system or network failure can cause the broker to be unavailable, making it impossible for the systems to operate normally. This is rarely a desirable situation.

For this reason, most message brokers enable logical queues and topics to be physically replicated across multiple brokers, each running on their own node. If one broker fails, then producers and consumers can continue to process messages using one of the replicas. This architecture is illustrated in Figure 7-4. Messages published to the leader are mirrored to the follower, and messages consumed from the leader are removed from the follower.

![](img/936a72197720a5158b16541313af61c9_178_0.png)

## Figure 7-4. Message Queue Replication

The most common approach to message queue replication is known as a leader-follower architecture. One broker is designated as the leader, and producers and consumers send and receive messages respectively from this leader. In the background, the leader replicates, or mirrors all messages it receives to the follower, and removes messages that are successfully delivered. This is shown in Figure 7-4 with the replicate and remove operations. How precisely this scheme is implemented and the effects it has

on broker performance is inherently implementation, and hence product dependent.

With leader-follower message replication, the follower is known as a hot standby, basically a replica of the leader that is available if the leader fails. In such a failure scenario, producers and consumers can continue to operate by switching over to accessing the follower. This is also called *fail over*. Fail over is implemented in the client libraries for the message broker, and hence occurs transparently to producers and consumers.

Implementing a broker that performs queue replication is a complicated affair. There are numerous subtle failure cases that the broker needs to handle when duplicating messages. I’ll start to raise these issues and describe some solutions in Chapters 10 and 11 when discussions turn to scalable data management.

> **NOTE**
> 
> Some advice - don’t contemplate *rolling your own* replication scheme, or any other complex distributed algorithm for that matter. The software world is littered with failed attempts to build application-specific distributed systems infrastructure, just because the solutions available ‘don’t do it quite right for our needs’ or ‘cost too much’. Trust me – your solution will not work as well as existing solutions and development will cost more than you could ever anticipate. And you will probably end up throwing your code away. These algorithms are really hard to implement correctly at scale.

## Example: RabbitMQ

RabbitMQ is one of the most widely utilized message brokers in distributed systems. You’ll encounter deployments in all application domains, from finance to telecommunications and building environment control systems. It was first released around 2009 and has developed into a fully featured, open source distributed message broker platform with support for building clients in most mainstream languages.

The RabbitMQ broker is built in Erlang, and primarily³ provides support for the Advanced Message Queuing Protocol (AMQP) open standard.

AMQP emerged from the finance industry as a cooperative protocol definition effort. It is a binary protocol, providing interoperability between different products that implement the protocol. Out of the box, RabbitMQ supports AMQP v0-9-1, with v1.0 support via a plugin.

## Messages, Exchanges, and Queues

In RabbitMQ, producers and consumers use a client API to send and receive messages from the broker. The broker provides the store-and-forward functionality for messages, which are processed in a FIFO manner using queues. The broker implements a messaging models based on a concept called exchanges, which provide a flexible mechanism for creating messaging topologies.

An exchange is an abstraction that receives messages from producers and delivers them to queues in the broker. Producers only ever write messages to an exchange. Messages contain a message payload, and various attributes known as message metadata. One element of this metadata is the *routing key*, which is a value used by the exchange to deliver messages to the intended queues.

Exchanges can be configured to deliver a message to one or more queues. The message delivery algorithm depends on the exchange type and rules called binding, which establish a relationship between an exchange and a queue using the routing key. The three most commonly used exchange types are shown in Table 7-1.

## Table 7-1. Exchange Types

| Exchange Type | Message Routing Behavior |
|---|---|
| Direct | Delivers a message to a queue based on matching the value of a routing key which is published with each message |
| Topic | Delivers a message to one or more queues based on matching the routing key and a pattern used to bind a queue to the exchange. |
| Fanout | Delivers a message to all queues that are bound to the exchange, and the routing key is ignored. |

Direct exchanges are typically used to deliver each message to one destination queue based on matching the routing key⁴. Topic exchanges are a more flexible mechanism based on pattern matching that can be used to implement sophisticated publish-subscribe messaging topologies. Fanout exchanges provide a simple one-to-many broadcast mechanism, in which every message is sent to all attached queues.

Figure 7-5 depicts how a direct exchange operates. Queues are bound to the exchange by consumers with three values, namely “France”, “Spain” and “Portugal”. When a message arrives from a publisher, the exchange uses the attached routing key to deliver the message to one of the three attached queues.

![](img/936a72197720a5158b16541313af61c9_183_0.png)

Figure 7-5. An Example of a RabbitMQ Direct Exchange

The following code shows an excerpt of how a direct exchange is configured and utilized in Java. RabbitMQ clients, namely producer and consumer processes, use a channel abstraction to establish communications with the broker (more on channels in the next section). The producer creates the exchange in the broker and publishes a message to the exchange with the routing key set to “France”. A consumer creates an anonymous queue in the broker, binds the queue to the exchange created by the publisher, and specifies that messages published with the routing key “France" should be delivered to this queue.

# Producer:

```
channel.exchangeDeclare(EXCHANGE_NAME, "direct");
channel.basicPublish(EXCHANGE_NAME, "France", null,
message.getBytes());
```

# Consumer:

```
String queueName = channel.queueDeclare().getQueue();
channel.queueBind(queueName, EXCHANGE_NAME, "France");
```

## Distribution and Concurrency

To get the most from RabbitMQ in terms of performance and scalability, you must understand how the platform works under the covers. The issues

of concern relate to how clients and the broker communicate, and how threads are managed.

Each RabbitMQ client connects to a broker using a RabbitMQ connection. This is basically an abstraction on top of TCP/IP, and can be secured using user credentials or TLS. Creating connections is a heavyweight operation, requiring multiple round trips between the client and server, and hence a single long-lived connection per client is the common usage pattern.

To send or receive messages, clients use the connection to create a RabbitMQ channel. Channels are a logical connection between a client and the broker, and only exist in the context of a RabbitMQconnection, as shown in the following code snippet

```
ConnectionFactory connFactory = new ConnectionFactory();
Connection rmqConn = connFactory.createConnection();
Channel channel = rmqConn.createChannel();
```

Multiple channels can be created in the same client to establish multiple logical broker connections. All communications over these channels are multiplexed over the same RabbitMQ (TCP) connection, as shown in Figure 7-6. Creating a channel requires a network round trip to the broker. Hence for performance reasons, channels should ideally be long lived, with channel churn, namely constantly creating and destroying channels, avoided.
