# 6
Events for Cloud-Native Integrations

# This chapter covers

+   - The power of Event-Driven architectures when building distributed systems
+   - How to produce and consume asynchronous events in a Kubernetes environment
+   - How events can help us to externalise what is happening in our services
+   - How events can be a perfect mechanism to implement integrations between disparate systems
+   - What tools can save you time when building Event-Driven applications on top of Kubernetes

So far, we have covered very basic service to service interactions using HTTP requests. HTTP requests are synchronous in nature, meaning that the receiving end must be up and running to be able to process the request correctly. While there are some mechanisms to retry HTTP requests if they fail, in real-life projects, this is not enough. Sometimes, you might want to understand what is going on inside your services using asynchronous mechanisms, or you might want to share information between multiple services without them knowing about each other or being up at the same time. A common way to decouple your services and implement asynchronous interactions is to use message queues. Where one service can produce messages and multiple services can be interested in consuming it in an asynchronous way.

If you start digging into message queues, sooner or later you will find out about Event-Driven architectures and how these tools can be used to build Event-based and reactive systems. In general, these architectures rely on the concept of “events.” An event can be thought of as something that has happened, which something else may be interested in. In these architectures, each service will be in charge of externalising what has happened internally by emitting events that other services can consume or react to them.

In this chapter, we will dig into how Event-Driven applications can be built on top of Kubernetes and what kind of tools can help us to ripe the benefits of more reactive