With Spring Cloud, four components are involved in publishing and consuming a message:

+   - Source
+   - Channel
+   - Binder
+   - Sink

When a service gets ready to publish a message, it will publish the message using a source. A source is a Spring-annotated interface that takes a Plain Old Java Object (POJO), which represents the message to be published. The source takes the message, serializes it (the default serialization is JSON), and publishes the message to a channel.

A channel is an abstraction over the queue that’s going to hold the message after it’s published by a message producer or consumed by a message consumer. In other words, we can describe a channel as a queue that sends and receives messages. A channel name is always associated with a target queue name, but that queue name is never directly exposed to the code. Instead, the channel name is used in the code, which means that we can switch the queues the channel reads or writes from by changing the application’s configuration, not the application’s code.

The binder is part of the Spring Cloud Stream framework. It’s the Spring code that talks to a specific message platform. The binder part of the Spring Cloud Stream framework allows us to work with messages without having to be exposed to platform-specific libraries and APIs for publishing and consuming messages.

In Spring Cloud Stream, when a service receives a message from a queue, it does it through a sink. A sink listens to a channel for incoming messages and deserializes the message back into a POJO object. From there, the message can be processed by the business logic of the Spring service.

# 10.3 Writing a simple message producer and consumer

Now that we’ve walked through the essential components in Spring Cloud Stream, let’s look at a simple Spring Cloud Stream example. For the first example, we’ll pass a message from our organization service to our licensing service, which prints a log message to the console. In addition, because we’ll only have one Spring Cloud Stream source (the message producer) and a sink (message consumer) in this example, we’ll start the example with a few simple Spring Cloud shortcuts. This will make setting up the source in the organization service and in a sink in the licensing service trivial. Figure 10.4 highlights the message producer and builds on the general Spring Cloud Stream architecture from figure 10.3.