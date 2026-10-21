hard and usually far away from developers. Cloud Providers are doing a fantastic job by bringing these topics closer to developers so they can be more autonomous and iterate faster. Unfortunately, when working with Kubernetes we have more options that we need to consider carefully to make sure that we understand the trade-offs. The next section covers how we can manage our Application Infrastructure inside Kubernetes; while this is usually not recommended, it can be practical for some scenarios.

# 4.1.1 Managing your own Application Infrastructure

Application infrastructure has become an exciting arena. With the rise of containers, every developer can bootstrap a database or a message broker with a couple of commands, and for development purposes, this is usually enough. In the Kubernetes world, this translates to Helm Charts, which uses containers to configure and provision Databases (relational and NoSQL), message brokers, identity management solutions, etc. As we saw in chapter 2 you installed the walking skeleton application containing 4 services with a single command. The same can be done for application infrastructure components.

For example, you can run `helm install postgresql bitnami/postgresql` to install an instance of PostgreSQL in your cluster. Same with Redis, Kafka and RabbitMQ. The amount of Helm charts available today is amazing, and it is quite easy to think that installing a Helm Chart will be the way to go.

```
helm install postgresql bitnami/postgresql
```

As discussed in Chapter 2 (Dealing with application state), if we want to scale our services that keep state we will need to provision specialized components such as databases. Application developers will define which kind of database will suit best depending on the data that they need to store and how that data will be structured. For our walking skeleton, we will be provisioning a Redis NoSQL database for the Agenda Service and a PostgreSQL database for the Call for Proposals (C4P) Service.