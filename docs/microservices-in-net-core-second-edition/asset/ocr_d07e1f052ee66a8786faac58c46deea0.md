the control of the microservice, and it’s exactly the data the microservice needs. First, it’s data that belongs to the capability the microservice implements, but it’s also supporting data, like cached data and read models created from event feeds.

The fact that each microservice owns a data store means you don’t need to use the same database technology for all microservices. You can choose a database technology that’s suited to the data that each microservice needs to store.

A microservice typically needs to store three types of data:

+   - Data belonging to the capability the microservice implements. This is data that the microservice is responsible for and must keep safe and up-to-date.
+   - Events raised by the microservice. During command processing, the microservice may need to raise events to inform the rest of the system about updates to the data the microservice is responsible for.
+   - Read models based on data in events from other microservices or occasionally on data from queries to other microservices.

These three types of data may be stored in different databases and even in different types of databases.

# 6.2 Partitioning data between microservices

When you’re deciding where to store data in a microservice system, competing forces are at play. The two main forces are data ownership and locality:

+   - *Ownership of data* means being responsible for keeping the data correct, safe, and up-to-date.
+   - *Locality of data* refers to where the data a microservice needs is stored. Often, the data should be stored nearby—preferably in the microservice itself.

These two forces may be at odds, and in order to satisfy both, you’ll often have to store data in several places. That’s okay, but it’s important that only one of those places be considered the authoritative source. Figure 6.1 illustrates that; whereas one microservice stores the authoritative copy of a piece of data, other microservices can mirror that data in their own data stores.

![](bbox=[0.18, 0.66, 0.85, 0.92])

Figure 6.1 Microservices A and C collaborate with microservice B. Microservices A and C can store mirrors of the data owned by microservice B, but the authoritative copy is stored in microservice B’s own data store.