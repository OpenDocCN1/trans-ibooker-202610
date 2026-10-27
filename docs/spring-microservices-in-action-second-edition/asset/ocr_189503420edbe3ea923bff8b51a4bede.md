# 3.2.2 Server or container sprawl

One of the most common deployment models for microservices is to have one microservice instance deployed in one container. In a large microservice-based application, you might end up with 50 to 100 servers or containers (usually virtual) that must be built and maintained in production alone. Even with the lower cost of running these services in the cloud, the operational complexity of managing and monitoring these services can be tremendous.

> NOTE The flexibility of microservices has to be weighed against the cost of running all of those servers. You can also have different alternatives, such as considering functional developments like lambdas or adding more microservice instances on the same server.

# 3.2.3 Application type

Microservices are geared toward reusability and are extremely useful for building large applications that need to be highly resilient and scalable. This is one of the reasons why so many cloud-based companies have adopted microservices. If you’re building small, departmental-level applications, or applications with a small user base, the complexity associated with building a distributed model like a microservice might generate more expense than it’s worth.

# 3.2.4 Data transactions and consistency

As you begin looking at microservices, you need to think through the data usage patterns of your services and service consumers. A microservice wraps around and abstracts away a small number of tables and works well as a mechanism for performing “operational” tasks like creating, adding, and performing simple (non-complex) queries against a data store.

If your application needs to do complex data aggregation or transformation across multiple data sources, the distributed nature of microservices will make this work difficult. Your microservices will invariably take on too much responsibility and can also become vulnerable to performance problems.

# 3.3 The developer’s tale: Building a microservice with Spring Boot and Java

In this section, we’ll explore the developer’s priorities in building the licensing microservice from the O-stock domain model.

> NOTE We created the skeleton of the licensing service in the previous chapter. In case you didn’t follow the code listings in that chapter, you can download the source code from https://github.com/ihuaylupo/manning-smia/tree/master/chapter2.