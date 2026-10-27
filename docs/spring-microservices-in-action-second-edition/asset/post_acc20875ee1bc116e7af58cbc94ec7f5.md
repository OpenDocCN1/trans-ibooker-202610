
# appendix A
Microservices
architecture best practices

In chapter 2, we explained some of the best practices that we should consider when creating a microservice. With this appendix we want to go deeper into these best practices. In the next sections, we will give you some guidelines (or best practices) to create a successful microservices architecture.

It is important to note that there isn’t a well-defined set of rules to achieve this; however, Hüseyin Babal in his talk “Ultimate Guide to Microservice Architecture” presented a set of best practices to achieve a flexible, efficient, and scalable design. We consider this talk essential among the developer’s community because it not only gives guidelines on how to develop a microservices architecture, but it also highlights which are the fundamental components of a microservice. In this appendix, we have selected some of the practices that we consider most important (those that we used throughout the book to create a flexible and successful architecture).

> **NOTE**  If you’re interested in knowing all of the best practices mentioned in the “Ultimate Guide to Microservice Architecture” talk, visit the following link that contains a video describing them: https://www.youtube.com/watch?v=CTnMUEO6oZI.

The following list gives some of the best practices mentioned in his talk. We’ll cover these in the following sections.

- Richardson Maturity Model
- Distributed configuration
- Monitoring
- Application performance management
- Spring HATEOAS
- Continuous delivery
- Logging
- API gateways

## Richardson Maturity Model

This best practice (http://martinfowler.com/articles/richardsonMaturityModel.html), described by Martin Fowler, is a guide to understanding the main principles of REST architectures and to evaluate our REST architecture. It’s important to note that we can see these levels more as a tool to help us understand the components of REST and the ideas behind the RESTful that specifies the action, the target, and the parameters to run the service.

For example, let’s assume that we want to buy a specific product from an online store. First, the store’s software needs to verify that is has the product in stock. At this level, the online store will expose a service endpoint with a single URI. Figure A.2 shows the level 0 service requests and responses.

![](img/3c6dc613426280ca3eafe43dea24391e_407_0.png)

Figure A.2 Level 0: a simple RPC with a plain old XML (POX) sent back and forth between consumer and service

![](img/3c6dc613426280ca3eafe43dea24391e_410_0.png)

**Figure A.6** Microservices CI/CD processes with a test and staging environment. In this figure, each team has its own source repository, unit and integration testing, image repository, and process of deployment. This allows each team to deploy and release newer versions without having to wait for other releases.

For the provisioning implementations, we need to make a technology shift. Spring framework(s) are geared toward application development and don’t have tools for creating a build/deployment pipeline.

## Monitoring

The monitoring process is critical when we talk about microservices. Microservices are part of large and complex distributed systems, and remember, the more distributed a system is, the more complicated it is to find and solve its problems. Imagine that we have an architecture with hundreds of microservices, and one of the microservices is affecting the performance of other services. How do we know which is the unstable service if we don’t implement a suitable monitoring process? In order to tackle this, in appendix C, we explain how to achieve a good monitoring system in our architecture using the following tools. Figure A.7 shows the interaction between Micrometer, Prometheus, and Grafana.

- Micrometer (https://micrometer.io/) is a default metrics library in Spring Boot 2 that allows us to obtain application metrics as well as JVM metrics like garbage collection, memory pools, and so forth.
- Prometheus (https://prometheus.io/) is an open source monitoring and alert system that stores all data as a time series.
- Grafana (https://grafana.com/) is an analysis platform for metrics that lets you query, visualize, and understand data no matter where it’s stored.

![](img/3c6dc613426280ca3eafe43dea24391e_411_0.png)

**Figure A.7**  Interaction between Micrometer, Prometheus, and Grafana

## Logging

Every time you think about logs think about *distributed tracing*. Distributed tracing helps us understand and debug our microservices by pinpointing where a failure occurs in a microservices architecture.

In chapter 11, we explained how to achieve this with Spring Cloud. The traceability of the requests we make within our architecture is another “new” technological (Spring Cloud Sleuth) situation that comes up when speaking about microservices. In monolith architectures, the majority of the requests that reach the application are resolved within the same application. The ones not following this rule were the ones that interacted with a database or some other service. However, in microservices architectures (Spring Cloud, for example), we find an environment composed of several applications, generating a single client request that can go through several applications until it is answered.

How can we follow the path of that request in a microservices architecture? We can associate a unique request identifier that can then be propagated across all the calls and add a central log collection to see all the entries of that client request.

## API gateways

An API gateway is an API REST interface that provides a central access point to a group of microservices and/or defined third-party APIs. In other words, the Spring Cloud API Gateway essentially decouples the interface that clients see from the microservice implementation. This is particularly useful when we want to avoid exposing internal services to external clients. Keep in mind that this system not only acts as a gateway, but you can also add additional features such as these:

- Authentication and authorization (OAuth2)
- Protection against threats (DoS, code injection, and so on)
- Analysis and supervision (who uses the APIs, when, and how)
- Monitoring of incoming and outgoing traffic

# appendix B
OAuth2 grant types

After reading chapter 9, you might think that OAuth2 doesn’t seem too complicated. After all, you have an authentication service that checks a user’s credentials and issues a token back to the user. The token can, in turn, be presented every time the user wants to call a service protected by the OAuth2 server.

With the interconnected nature of the web and cloud-based applications, users have come to expect that they can securely share their data and integrate functionality between different applications owned by various services. This presents a unique challenge from a security perspective because you want to integrate across different applications while not forcing users to share their credentials with each application they want to integrate with.

OAuth2 is a flexible authorization framework that provides multiple mechanisms for applications to authenticate and authorize users without forcing them to share credentials. Unfortunately, it’s also one of the reasons why OAuth2 is considered complicated. These authentication mechanisms are called *authentication grants*. OAuth2 has four forms of authentication grants that client applications can use to authenticate users, receive an access token, and then to validate that token. These grant types are

- Password
- Client credential
- Authorization code
- Implicit

In the following sections, we walk through the activities that occur during the execution of each of these OAuth2 grant flows. We also talk about when to use one grant type over another.
