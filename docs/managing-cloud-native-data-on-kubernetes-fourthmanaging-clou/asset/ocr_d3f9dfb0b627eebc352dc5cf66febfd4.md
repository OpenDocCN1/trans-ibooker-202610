![](bbox=[0.14, 0.09, 0.86, 0.48])

Figure 9-4. Deploying Milvus in Kubernetes

The Access Layer contains the Proxy Service, which uses a Kubernetes Load Balancer to route requests from client applications. The services in the Coordination Layer handle incoming search and index queries, routing them to the core server components in the Worker Layer that handle queries and manage data storage and indexing. The Data Nodes manage persistence via files in object storage. The Message Storage uses Apache Pulsar or Apache Kafka to store the stream of incoming data that is then passed to Data Nodes.

As you can see, Milvus is designed to be Kubernetes native, with a horizontally scalable architecture that makes it well poised to scale up to massive data sets including billions or even trillions of vectors.

# Efficient Data Movement with Apache Arrow