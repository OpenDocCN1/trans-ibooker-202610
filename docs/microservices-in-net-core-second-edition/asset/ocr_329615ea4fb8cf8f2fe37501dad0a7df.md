![](bbox=[0.194, 0.083, 0.887, 0.374])

**Figure 11.4** The microservice platform is a collection of implementations of technical concerns that cut across all microservices. The platform is installed in—and thus becomes part of—every microservice.

the platform is installed in every microservice. Later in this chapter, you’ll create a first iteration of such a platform, including the following:

+   - Monitoring endpoints that fit into Kubernetes
+   - Logging configuration that streamlines structured logging and tracing

These are the platform areas I’ve chosen to cover for this book, but they aren’t necessarily the things you’ll need in a microservice platform for your system. I’ve tried to include common components. In particular, I think such platforms should include the monitoring, tracing, and structured logging bits. They don’t have to be in the exact form used in this book; you may, for instance, prefer a different tracing standard or another logging framework. Depending on the approach you choose to use for security between microservices, you may also have an opportunity to make security easier by putting cross-cutting security code in the platform. Furthermore, you should consider including policies related to technologies you use in many microservices—for example, handling database connections.

The other side of the coin is this: should you include more in your microservice platform? There’s no overarching answer, but you should be cautious about adding items; every time you include something in the platform, it becomes a little bigger and heavier. In addition, when you add something that all microservices are expected to include, what happens if they aren’t running the same version of the platform? Can other microservices handle that? Can the infrastructure around your microservice system handle that? If the platform grows into something that must be in sync across all