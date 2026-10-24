The event-subscriber process is a background process that periodically makes requests to the event feed on the special offers microservice to get new events. When it gets back a batch of new events, it processes them by sending commands to the notifications microservice to notify registered users about new special offers. The SpecialOffersSubscriber component is where the polling of the event feed is implemented, and the Notifications-Client component is responsible for sending the command to notifications. The rate at which the event subscribes gets events can differ quite a bit based on the specific use case. Often, events are fetched every 30 seconds or every few minutes, but there are also cases where fetching events occur frequently, like every second, or where fetching events seldom occur, like every hour, and is the right thing to do.

This is the way you implement event subscriptions: microservices that need to subscribe to events have a subscriber process with a component that polls the event feed. When new events are returned from the event feed, the subscriber process handles the events based on business rules.

# Events over queues

An alternative to publishing events over an event feed is to use a queue technology, like RabbitMQ, AWS SQS, or Azure Queue Storage. In this approach, microservices that publish events push them to a queue, and subscribers read them from the queue. Events must be routed from the publisher to multiple subscribers, and how that’s done depends on the choice of queue technology and may require additional technologies on top of the queue. As with the event-feed approach, the microservice subscribing to events has an event-subscriber process that reads events from the queue and processes them. Event replay can also be built with queues but may require a bit of extra work depending on the queue technology.

This is a viable approach to implementing event-based collaboration between microservices. But this book uses HTTP-based event feeds for event-based collaboration because it allows events to be replayed at any time and it’s a simple yet robust and scalable solution.

# Event over gRPC

As mentioned, command and queries map directly to gRPC’s remote procedure calls. But what about events? gRPC has a concept of streaming that allows for keeping an HTTP/2 connection open and sending data through the connection. Streaming in gRPC works in both directions—from client to server or from server to client. To model events using gRPC streaming, you can create a remote procedure that works somewhat like an event feed: it allows event subscribers to call the procedure and get events back in a response stream.

# 5.1.3 Data formats

So far, we’ve focused on exchanging data in JSON format. JSON works well in many situations, but there are reasons you might want to use something else:

+   If you need to exchange a lot of data, a more compact format may be needed. Text-based formats such as JSON and XML are a lot more verbose than binary formats like protocol buffers.