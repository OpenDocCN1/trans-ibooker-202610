![](bbox=[0.145, 0.076, 0.853, 0.383])

Figure 3-12. Compacting older events of the same key due to updated values or tombstones

# Selecting an Event Broker

Selecting a suitable event broker that supports the Kappa architecture pattern is essential for creating a self-service data mesh. Both ephemeral message-passing and queues are insufficient to underpin a data mesh—you will need to ensure your event broker supports event streams in the form of durable append-only logs.

To select a broker, you need to focus on the essential requirements and make your own informed decision. These include:

# Unlimited durable data capacity

You must be able to store all pertinent events for as long as they are relevant. For most data products, you’ll need to store them indefinitely. Data product consumers must be able to access and replay the events whenever they want.

# Scalability

Though scalability is largely a given in the era of cloud computing, you need to ensure that your event broker can scale up to extremely high throughputs and number of clients. Additionally, the event broker must be able to serve the stored data with high performance, as data products follow a write-one, read-many-times access pattern.