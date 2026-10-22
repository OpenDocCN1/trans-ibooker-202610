Feature scope is likely the most important selection criteria when evaluating which streaming technology to use. Still, you should also challenge yourself to add the suitability for Kubernetes as a criteria and consider whether more complex features are worth the added resource cost. Fortunately, the price for getting your decision wrong the first time is relatively low. Streaming data systems tend to be some of the easiest to migrate due to their ephemeral nature. The deeper into your feature stack the streaming technology goes, the harder it is to move. The scope of streaming features can be broken into the two large buckets shown in Figure 8-3.

![](bbox=[0.2, 0.33, 0.78, 0.54])

Figure 7-3. Streaming Types

# Message broker

The simplest form of streaming technology that facilitates the moving of data from one point to another with one or more of the delivery methods and guarantees listed above. It’s easy to discount this feature’s simplistic appearance, but it’s the backbone of modern cloud native applications. It’s like saying FedEx is just a package delivery company, but imagine what would happen to the world economy if it stopped for even one day? Example OSS message brokers include Apache Kafka, Apache Pulsar, RabbitMQ, and Apache ActiveMQ.

# Stream analytics