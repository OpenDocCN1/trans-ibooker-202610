# An Overview of Popular Cloud Native Messaging Platforms

In this section, we will review a number of popular cloud native messaging platforms that have become commonplace in modern infrastructure.

# RabbitMQ

Although RabbitMQ is more than 15 years old and obviously predates widespread public adoption of the cloud and microservices, it is a proven messaging system that has powered critical workloads and has continued to be relevant in the world of containers and Kubernetes clusters. One of the reasons for this is that RabbitMQ was developed in Erlang/OTP, a functional programming language that is designed for scalability, concurrency, and reliability and is therefore ideal for building distributed systems that process millions to hundreds of millions of messages per day with very low downtime.

# Apache Kafka

In 2010, engineers at LinkedIn found that RabbitMQ could not scale to the complex nature and size of their ecosystem. Kafka was created to allow messaging to become the core of LinkedIn’s technology stack and has successfully scaled to trillions of messages a day across the site.

Later in this chapter we will discuss Azure Event Hubs, a service that is compatible with Apache Kafka and allows applications to simply update the consumer/producer connection string, but may not make all of Kafka’s features available. Managed Kafka is also part of HDInsight, a broad suite of analytics services in the cloud on Azure. HDInsight is beyond the scope of this book.

# CNCF CloudEvents

CloudEvents was first started in 2017 within the CNCF Serverless Working Group. In CNCF’s own words: “CloudEvents is a specification for describing event data in common formats to provide interoperability across services, platforms and systems.” While this project does have large potential (and is supported by Azure Event Grids), we won’t cover it in further detail in this book.

# Cloud Messaging Deep Dive with NATS

NATS is another open source messaging implementation that has been accepted by CNCF. NATS was first released in 2011 and was initially implemented in Ruby as the messaging and service discovery layer for Cloud Foundry. It was later rewritten in Golang and is under very active development today. Although not as popular as RabbitMQ or Kafka, NATS provides multiple easily deployable options due to its single binary server available on multiple operating systems, processor architecture, and official Docker repositories. Client libraries are available for all popular programming languages. Simplicity, operability, performance, and security are cited as the advantages of NATS over other messaging implementations, as is the ability to easily use a bare minimum feature set with a preference for “at most once” delivery.

Since the base pub/sub protocol does not implement message persistence, consumers must be active to receive the message, making NATS optimal for exchanging data that is highly perishable, and data that is frequently updated such as sensor data. By default, messages will be routed to all listening subscribers to that topic, similar to multicast networking.

Given its low memory footprint and extremely high performance, NATS is also ideal for edge and IoT analytics use cases. For such a lightweight messaging implementation that focuses on simplicity, NATS has relatively sophisticated authentication and authorization capabilities and supports multitenancy by default through accounts. NATS Streams and NATS JetStream support both memory and disk-based persistence. NATS pub/sub and streaming have supported high availability and horizontal scalability from the start, and JetStream has been implementing them since early 2021.