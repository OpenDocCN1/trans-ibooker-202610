![](bbox=[0.36, 0.09, 0.62, 0.28])

Figure 10-4. The CNCF Streaming & Message Landscape

# Messaging Fundamentals

Understanding the similarities and differences between contemporary messaging solutions can help you determine which tool is best for the job or the problem at hand. As we have seen throughout this book, there is always more than one way to build something, and it is not always an easy decision to pick the right path until advertised capabilities of the tool are tested and validated with your specific infrastructure and use case. Before we explore how RabbitMQ, Apache Kafka, and NATS fit into the cloud native infrastructure and applications, we must understand the basic concepts and common capabilities in messaging, queuing, and streaming systems.

# Producers and Consumers

Just as when you are communicating with another human, a message always has a sender and a receiver, but the message may or may not be sent directly and you may or may not get a response. When you send an email, there are multiple intermediate parties (see Figure 10-5). These parties include your email client, your email provider’s servers (message transfer agents [MTAs], which play a broker role), the receiver’s server (another MTA), and a receiving email client. In this example, there are a number of consumers and producers; the email client queues messages to send/produce to the ISP’s mail servers, and the MTA then queues the sent messages and sends/produces them to the receiver’s MTA. The receiver’s email client will then receive/consume the messages from their ISP’s MTA.

![](bbox=[0.25, 0.62, 0.73, 0.77])

Figure 10-5. The SMTP mail protocol, an example of a queueing system

Just as in the case of networking where there are unicast, multicast, and broadcast delivery patterns, in messaging there may be cases where a single consumer processes data from a single producer or multiple consumers (like an email). Messages may be pushed to or pulled by consumers depending on the platform and configuration.

# Brokers and Clustering