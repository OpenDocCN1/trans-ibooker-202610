![](bbox=[0.16, 0.12, 0.84, 0.41])

Figure 7-2. Persisting Messages to Disk

Persistent queues have an inherent increase in the response time for send operations, with the tradeoff being enhanced data safety. Brokers will usually maintain the queue contents in memory as well as on disk so messages can be delivered to consumers with minimal overhead during normal operations.

# Publish-Subscribe

Message queues deliver each message to exactly one consumer. For many use cases, this is exactly what you want—my online purchase return needs to be consumed just once by the originating vendor—so that I get my money back.

Let’s extend this use case. Assume the online retailer wants to do analysis of all purchase returns so it can detect vendors who have a high rate of returns and take some remedial action. To implement this, you could simply deliver all purchase return messages to the respective vendor and the new analysis service. This creates a one-to-many messaging requirement, which is known as a publish-subscribe architecture pattern. In publish-subscribe