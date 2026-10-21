To avoid defining what an Event is and how you can encode these events when using different protocols (HTTP, AMQP, AVRO, etc), the CloudEvents specification ( http://cloudevents.io ) was created.

# WHAT ARE CLOUDEVENTS? AND WHY DO I NEED THEM?

![](bbox=[0.18, 0.35, 0.88, 0.65])

Figure 6.5 CloudEvent bindings to AMQP and HTTP

As shown in figure 6.5, CloudEvents are just a thin wrapper adding metadata about our application-specific events. But this wrapper becomes essential for us to build robust solutions that are transport agnostic. Later in this chapter, we will see CloudEvents in action. Let’s now switch to our Realm: Kubernetes.

# 6.1.3 Producers, Consumers and Events on Kubernetes

Unfortunately, none of the concepts (abstractions) introduced in Chapter 2, such as Services, Deployments, Pods, Ingresses, are designed to work with Events. There is no concept of Deployment or Pods producing or consuming events, neither event routers nor event filters.

Services and Deployments in Kubernetes are just abstractions that we can use to compose our Cloud-Native applications. If our applications or some features follow the Event-