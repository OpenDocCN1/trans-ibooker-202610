approaches. This chapter builds upon the previous chapters by adding producers and consumers of events to implement a new flow inside the Conference Platform application. The content of this chapter is divided into three main sections:

+   - Producers and Consumers of Events
+     - Why would you use a Message Queue?
+     - CloudEvents, what are they?
+     - How do we work with Producers and Consumers in Kubernetes?
+   - Building Event-Driven applications using Knative Eventing
+     - Decoupling components and relying on CloudEvents for system-to-system integrations
+   - Selling Tickets for a very popular event
+     - Building an Event-Driven flow using CloudEvents

We will start simple, by looking at Producers and consumers of events, the basic building blocks for Event-Driven architectures, so then we can look at how these concepts can be implemented with real-life tools and frameworks on top of Kubernetes.

# 6.1 Producers and Consumers of events

In this section we are going to cover the underlying principles behind Event-Driven architectures, how they relate with message queues and message brokers, the need for a standard to encode our events and how all these topics map to the Kubernetes world.

If you were ever tasked with integrating multiple systems together you are probably very familiar with the concepts of Producers and Consumers of data, messages or events. If you are not, I would recommend you check the Enterprise Integration Pattern website and book for more details (https://www.enterpriseintegrationpatterns.com/). While producers and consumers can be used for integration purposes, they can be also used to architect more reactive applications. For this reason, I will not spend much time covering the basics, I will focus on the benefits so then we can look at how we can rely on these concepts when architecting and building our Cloud Native applications.